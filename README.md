# Chandrasheel-Prasad-Front-End
Q1. Explain what the simple List component does.

Ans-> 

The List component is a basic building block of many user interfaces that displays a list of items vertically stacked on top of each other.

Here is an example code snippet of a List component in React:
```
import React from 'react';

function MyList() {
  const items = [
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' },
    { id: 3, name: 'Item 3' }
  ];

  return (
    <ul>
      {items.map(item => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```

-------------------------------------------------------------------------------------------------------


Q2. What problems / warnings are there with code?

Ans->

So the following are the error that i encountered while going thru the given code:

Error 1: In the Single List Item Component, the onClickHandler Function should be passed as a callback instead of begin invoked immediately. So the onClick attribute should be : 
```
onClick={() => onClickHandler(index)}
```

Error 2: In the List component, the useState hook should be called with a default value of null, like this :
```
 const [selectedIndex, setSelectedIndex] = useState(null);
```
 Error 3: In the List component, the propTypes for the items prop should be defined as follows:
```
 WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};
```
Error 4: In the list component, the defaultProps for the items prop should be defined as follows:
```
WrappedListComponent.defaultProps = {
  items: [],
};
```
Error 5: In the list component, the isSelected prop passed to SingleListItem should be a boolean value indicating whether the current item is selected or not.
```
 isSelected={selectedIndex === index}
```
---------------------------------------------------------------------------------------------------
```
Q3. Please fix, optimize, and/or modify the component as much as you think is necessary.


Ans-> 


import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? "blue" : "grey" }}
      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: "left" }}>
      {items &&
        items.map((item, index) => (
          <SingleListItem
            key={index}
            onClickHandler={() => handleClick(index)}
            text={item.text}
            index={index}
            isSelected={selectedIndex === index}
          />
        ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};

WrappedListComponent.defaultProps = {
  items: [],
};

const List = memo(WrappedListComponent);

// App Component
const App = () => {
  const [animals] = useState([
    { text: "Lion" },
    { text: "Tiger" },
    { text: "Bear" },
    { text: "Elephant" },
    { text: "Giraffe" },
  ]);

  return (
    <div>
      <h1>Animal List</h1>
      <List items={animals} />
    </div>
  );
};

export default App;
```