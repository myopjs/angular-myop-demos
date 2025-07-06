# Myop Angular Integration Guide

## Overview
This guide explains how to integrate your Angular components with Myop, allowing you to expose them as web components and incorporate them into Myop flows.

## Table of Contents
- [Exposing Angular Components](#exposing-angular-components)
- [Creating Myop WebComponents](#creating-myop-webcomponents)
- [Creating Flows](#creating-flows)
- [Using Myop Components in Angular](#using-myop-components-in-angular)
- [Communication with Angular Components](#communication-with-angular-components)

## Exposing Angular Components

There are two ways to expose your Angular components to Myop:

### Direct Exposure
Use the `expose` function to directly expose your Angular component:

```typescript
import {appConfig} from './app/app.config';
import {AppComponent} from './app/app.component';
import {exposeAngularComponent} from "@myop-dev/angular-remote";

exposeAngularComponent(AppComponent, 'test-comp-1', async () => {
   return appConfig
}).then();
```

## Creating Myop WebComponents

Follow these steps to create a Myop WebComponent from your Angular component:

1. Open the Myop dashboard at [https://dashboard.myop.dev](https://dashboard.myop.dev) <br>
   <img src="docs/images/dashboard.png" alt="Dashboard" width="500"><br>

2. To create a new component using a predefined template, click on the "Use sample component" button.
3. To create a new component from scratch, click on the "Create from scratch" button.
4. Choose Angular loader type <br/>
   <img src="docs/images/loader-selection.png" alt="Loader Selection" width="500"><br/>

5. Define how this component connects to your host app using props and refs. <br/>
6. If you're using a template, you can add all detected refs. Otherwise, add your own refs / props from the configuration panel.
   <img src="docs/images/add-detected-refs.png" alt="Add detected refs" width="500"><br/>
   <img src="docs/images/add-ref.png" alt="Add detected refs" width="500"><br/>


7. Configure your component:
   - Enter the component name and description.
   - Enter your component URL (e.g., http://localhost:4400/main.js).
   - Enter the tag name that you exposed the component with.
   - Click on the "view source code" button to view the React code example .
   - Click on the "Set to default" to set the relevant variant as the default one.
   - Click on the Delete button to delete a variant. <br/>
     <img src="docs/images/variants-section.png" alt="Variants Section" width="600" height="100"><br/>


**Note:** The GUID in the browser URL is the `componentId` that you'll need later

8. Saving:
   - If you want to save the component, click on the "Save" button
   - If you want to attach the component to an existing "Flow", Click on the ChevronDown button to search for the relevant flow
   - If you want to attach the component to a new flow, click on the (after the component itself was already saved). <br/>
     <img src="docs/images/saving.png" alt="Saving" width="300"><br/>

9. The component is saved and published! You can copy the code snippet and add this to your host application. <br/>
   <img src="docs/images/saved-component.png" alt="Saved" width="400"><br/>


## Creating Flows

1. Open the Myop dashboard at [https://dashboard.myop.dev](https://dashboard.myop.dev)
   <img src="docs/images/dashboard.png" alt="Dashboard" width="500"><br/>

2. Click on the "Create a new flow" button to add a new flow

3. Add your component to the flow <br/>
   <img src="docs/images/flow.png" alt="Saving" width="400"><br/>


4. Note: The GUID in the browser URL is the `flowId` that you'll need later
## Using Myop Components in Angular

You can integrate Myop components into your Angular application in two ways:

### Option 1: Via Routing

```typescript
// Before
import { TodoComponent } from './todo.component.ts';

{ path: 'todo', component: TodoComponent }

// After
import { MyopContainerComponent } from '@myop-dev/angular';

{
    path: 'todo',
    data: {
      componentId: "ca8c0c4f-d26e-40c8-bf32-19eb104ee710",
      flowId: "1d75e2f9-9a2d-49f1-aeeb-6268921a29fe"
    },
    component: MyopContainerComponent
}
```

### Option 2: As a Component in Templates

```typescript
// Before
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router';
import { TranslocoRootModule } from './transloco-root.module';
import { TodoComponent } from './todo.component.ts';

@Component({
   selector: 'app-root',
   imports: [TodoComponent],
   template: '<todo-component></todo-component>',
})
export class AppComponent {
 
}

// After
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router';
import { TranslocoRootModule } from './transloco-root.module';
import { MyopContainerComponent } from '@myop/angular';

@Component({
   selector: 'app-root',
   imports: [MyopContainerComponent],
   template: `<myop-container
                  flowId="1d75e2f9-9a2d-49f1-aeeb-6268921a29fe"
                  componentId="4df90a03-553c-44a3-b153-d0ddccdc0010"/>`
})
export class AppComponent {

}
```

## Communication with Angular Components

The `MyopContainer` component accepts `inputs`, which can include a wide variety of data types. This may consist of simple primitive values, complex objects, functions, and even non-serializable objects.

The `MyopContainer` component also provides a `componentReady` event that you can use to communicate with your Angular component.

```html
<myop-container
   flowId="1d75e2f9-9a2d-49f1-aeeb-6268921a29fe"
   componentId="4df90a03-553c-44a3-b153-d0ddccdc0010"
   [inputs]="someObject"
   (componentReady)="onReady($event)"
 />
```
In your component class
```typescript

someObject = {
   key1: '123',
   key2: '456',
   someInputValue : 'testValue',
   fun1 : ()=>{
      alert('ffff')
   }
}

onReady(component: IMyopComponent) {
    // Send messages to the component
    component.send(ChangeTextMessage.create(component.refs.title, this.inputs.name));
    
    // Show the component
    component.show();
}
```

## Additional Resources

- For more information on the Myop platform, visit [https://myop.dev/](https://myop.dev/)
- For API documentation, refer to [https://docs.myop.dev/](https://docs.myop.dev/)
