Let's say you have a parent component that consists of two child components, of which one is only visibile. 

There are different ways to approach:
1.) ***Have a seperate route*** Is an option if both components are not using similar data. So let's say you have a form component and a form-review component. If you directly would go to form-review, without first filling in the form, you would end end up in an empty page - which is obviously not what you want.
2.) ***Redux*** Update state in Redux store; require you to action creator, set-up reducer, pull in piece of staste.
3.) ***Have a component piece of state ***
