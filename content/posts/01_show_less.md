---
title: Show Less/More
description: This is the second post.
date: 2024-06-01
tags:
  - React
  - Trial
  - Another
---

I wanted to create a *accordion* like show/hide functionality to my React project. It was a simple enough request from client (me!), but it was not a simple search for my part.

First of all, the things I wanted to show was an array full of objects and the initial rendering was made by a `.map` method.

```jsx
const arrayName = [
  {title: "",description:"",url:""},
  {title: "",description:"",url: ""},
  ...
  ]
  
return(

...

{arrayName.map((arrayItem) => (
          <Component
            key={arrayItem.title}
            title={arrayItemt.title}
            description={arrayItem.description}
            url={arrayItem.url}
          />
        ))})
  
```

I didn't wanted to change the whole code just for this. So I tried to find a solution around the code.

My first idea was to limit the viewport, hide the overflow with some css and connect the button to viewport to widen the part where my list is rendered. It was simple, fast and first to come to mind.

But it has caveats of its own. First of all it would be a responsiveness hell. Also, it was all or limited, maybe in the future I wanted a more gradual system like show more> show more> show more... show less/collapse. So it was a good oppurtunity to create a `useState` hook.

So I defined the useState array at the start of the main function:

```js
const [itemList, setItemList] = useState(3);
```

I preferred the conventional naming for everything. It actually doesn't matter what you name them, but for readability I chose the boring names.  
for the initial state I wrote '3' because that's the number of items I wanted to show, well... 'initially'.

So now the imporant part comes. To find this solution took some time on my part.  
As I mentioned, rendered array used a `.map` method. Which maps *all* the items in array. How can I show less, and more importantly, then change the number to show more?

Well the answer is another method called `.slice`.

And in JavaScript you can use more than one method on the same line;

```js
{arrayName.slice(0, itemList).map((arrayItem) => (
          <Component
            key={arrayItem.title}
            title={arrayItemt.title}
            description={arrayItem.description}
            url={arrayItem.url}
            imageSrc={arrayItem.imageSrc}
          />
        ))}
```

It takes two arguments the start and end point of a 'slice' of your array. Since I wanted to start from the begining all the time first one is '0', second was the 'itemList' and this way I can change it anytime I want.

then I added a function to manipulate the state:

```js
function handleClick() {
    if (itemList == 3) {
      setItemList(arrayName.length);  
    } else {
      setItemList(3);
    }
  }
```

It is pretty straight foward but I'll explain anyway. When clicked if the itemlist in initial state which is 3, change the state to array's length, which means all of it.  
When state is changed, and become more than 3 else is run when we call the function, which changes the state to 3 again.  
(This comparison probably be made with absolute compare `===` better. But it works all the same in this case.)

And lastly a Button to bind the function added to the rendering part:

```jsx
return(
    
    ...
    
<Button onClick={handleClick}></Button>
)
```

Now, This buttons text needs to change to 'more' or 'less'. And we can do it with a simple ternary script here as well.

```jsx

condition ? exprIfTrue : exprIfFalse

```


Another way that we can do it with is to add this to `handleClick` and pass it to rendering parts. But I have other plans for there later, so this will do fine. so in our case it will be like this;

```jsx
return(
    ...
    ...
<Button onClick={handleClick}>
    {itemsList === 3 ? "Show More" : "Show Less"}
</Button>
)
```

**Sucsess!** Now with useState I made a functional button that shows More/Less.

But now the position of viewport changes and leaves me in unwanted places everytime I press the button. So it would be nice to add some animation and change the position as well. So I expand my `handleClick` function a little more;

```jsx
  const [itemList, setItemList] = useState(3);
  const startElement = document.getElementById("section-start");
  const endElement = document.getElementById("section-end");
  
function handleClick() {
    if (itemList == 3) {
      setItemList(projects.length);
      endElement.scrollIntoView({
        behavior: "smooth",
        block: "start",
      });
    } else {
      setItemList(3);
      startElement.scrollIntoView({
        behavior: "smooth",
        block: "start",
      });
    }
  }
```

with `getElementById` I added some `id` to places I wanted to scroll and locate them in my function.
then with `.scrollIntoView` I change my viewport everytime I pressed the button. 