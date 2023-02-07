<h1>Introduction</h1>
<p>The following example shows how to build a dynamic load component when click on button its added to the DOM and remove other components (like chatbot options).

</p>

<h1>Dynamic Render Component</h1>
<h2>TS Code</h2>


```typescript 
export class ParentComponent{
    // tslint:disable-next-line: completed-docs
    @ViewChild('viewContainerRef', { read: ViewContainerRef })
    public vcr!: ViewContainerRef;
    /** componentsList contains component and containerId */
    public componentsList = {
        componentOne: FirstComponent
        componentTwo: SecondComponent
        componentThree: ThirdComponent
    };
 /** component refrence array */
 public ref = [];
 /** component unique key */
 public childUniqueKey: number = 0;
/** childComponentRef */
public childComponentRef:  ComponentRef<any>;
/**
 *
 * @param document
 */
 constructor (@Inject(DOCUMENT) private document: any,
 ) {}
/** AddComponent
* @param refrence component
* @param vcr ViewContainerRef
* creates component refrence and added to refrence array
* removes all previous components from array
* push new component in pased view containerRef
*/

public AddComponent (component?, vcr?: ViewContainerRef) {
    if (component) {
      this.childUniqueKey && this.RemoveComponent(this.childUniqueKey, vcr);
      this.childComponentRef = vcr.createComponent(component);
      const childComponent = this.childComponentRef.instance;
      childComponent.index = ++this.childUniqueKey;
      this.ref.push(this.childComponentRef);
    }
 }
    /** AddSelectedComponent
     * clickEvent on button that calls add component function
     * call add component function
     */
    public AddSelectedComponent (clickedComponent) {
    
        this.AddComponent(
            this.componentsList[clickedComponent],
            this.vcr,
        );
    }
  

/** RemoveComponent
* @param index of previous component
* @param vcr ViewContainerRef
* removes component from refrence array and hostview by index
*/
public RemoveComponent (index: number, vcr: ViewContainerRef) {

 if (vcr.length < 1) {
   return;
 }

 const componentRef = this.ref.find(x => x.instance.index == index);
 const vcrIndex: number = componentRef && vcr.indexOf(componentRef.hostView);
 // removing component from container
 vcr.remove(vcrIndex);

 this.ref = this.ref.filter(x => x.instance.index !== index);
}
}
```

<h2>HTML</h2>

```html
<button AddSelectedComponent('componentOne')>Add first Component</button>
<button AddSelectedComponent('componentTwo')>Add Second Component</button>
<button AddSelectedComponent('componentThree')>Add Third Component</button>

<ng-template #viewContainerRef></ng-template>
```
<h1>Conclusion</h1>
<p>Add dynamic Component is used to develop something like chatbot when user click on a button it add component to dom and when click the other option it removes the previous component and add the new one</p>
