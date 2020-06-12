# testDome-react-anwser
### solutions to testdome.com practice React Interview Questions

### 1.Toggle Message
```
class Message extends React.Component {
	constructor(props) {
  super(props);
  this.state = {status: false};
  }
  
  handleOnClick() {
    this.setState((prevState) => ({
    	status: !prevState.status
    }));
  }
  render() {
    return (<React.Fragment>
          <a onClick={() => this.handleOnClick()} href="#">Want to buy a new car?</a>
          {this.state.status ? <p>Call +11 22 33 44 now!</p> : null}
        </React.Fragment>);
  }
}

document.body.innerHTML = "<div id='root'> </div>";
  
const rootElement = document.getElementById("root");
ReactDOM.render(<Message />, rootElement);

```

### 2.Focus
```
class Input extends React.PureComponent {
  render() {
    let {forwardedRef, ...otherProps} = this.props; 
    return <input {...otherProps} ref={forwardedRef} />;
  }
}

const TextInput = React.forwardRef((props, ref) => {
  return <Input {...props} forwardedRef={ref} />
});

class FocusableInput extends React.Component {
  constructor(props){
    super(props);
    this.ref = React.createRef()
  }
  
 
  render() {
    return <TextInput ref={this.ref} />;
  }

  // When the focused prop is changed from false to true, 
  // and the input is not focused, it should receive focus.
  // If focused prop is true, the input should receive the focus.
  // Implement your solution below:
  componentDidUpdate(prevProps) {
    if(!prevProps.focused && this.props.focused && !this.ref.current.hasFocus){
      this.ref.current.focus();
    }
  }
  
  componentDidMount() {
    if(this.props.focused) {
      this.ref.current.focus();
    }
  }
}

FocusableInput.defaultProps = {
  focused: false
};

const App = (props) => <FocusableInput focused={props.focused} />;

document.body.innerHTML = "<div id='root'></div>";
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

### 3.Change Username
```
class Username extends React.Component {
  state = { value: "" };

  changeValue(value) {
    this.setState({ value });
  }

  render() {
    const { value } = this.state;
    return <h1>{value}</h1>;
  }
}

function App() {

  const textInput = React.useRef(null);
  const nameValue = React.useRef(null);
  
  function clickHandler() {
  	nameValue.current.changeValue( textInput.current.value);
  }

  return (
    <div>
      <button onClick={clickHandler}>Change Username</button>
      <input ref={textInput} type="text" />
      <Username ref={nameValue} />
    </div>
  );
}

document.body.innerHTML = "<div id='root'></div>";
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

### 4. Grocery App
```
const Product = props => {
  const plus = () => {
   props.onVote(1, props.ind);
  };
  const minus = () => {
   props.onVote(-1, props.ind);
  };
  return (
    <li>
      <span>{props.product.name}</span> - <span>votes: {props.product.votes}</span>
      <button onClick={plus}>+</button>{" "}
      <button onClick={minus}>-</button>
    </li>
  );
};

const GroceryApp = (props) => {
  let [products, setProducts] = React.useState(props.products);
  const onVote = (dir, index) => {
  	const newProducts = [...products];
    newProducts[index].votes += dir
   	setProducts(newProducts);
  };

  return (
    <ul>
      {products.map((item, index) => 
      <Product 
        product={item}
        onVote={onVote}
        ind={index}
       />)}
    </ul>
  );
}

document.body.innerHTML = "<div id='root'></div>";

ReactDOM.render(<GroceryApp
  products={[
    { name: "Oranges", votes: 0 },
    { name: "Bananas", votes: 0 }
  ]}
/>, document.getElementById('root'));

let plusButton = document.querySelector("ul > li > button");
if(plusButton) {
  plusButton.click();
}
console.log(document.getElementById('root').outerHTML)
```

### 5. Image Gallery App
```
class ImageGallery extends React.Component {
	state = { links: this.props.links };
	
  handleOnClick(ind) {
  	event.preventDefault();
  	let newLinks =[...this.state.links];
    newLinks.splice(ind, 1);
    this.setState({ links : newLinks});
  }
  
  render() {
    return (
    	<div>
    	  {this.state.links.map((link,index) =>  
        <div class="image">
        <img src={link} />
        <button onClick={() => this.handleOnClick(index)} class="remove">X</button>
        </div>)}
      </div>
    );
  }
}

document.body.innerHTML = "<div id='root'> </div>";
  
const rootElement = document.getElementById("root");
const links = ["https://goo.gl/kjzfbE", "https://goo.gl/d2JncW"];
ReactDOM.render(<ImageGallery links={ links } />,
                rootElement);
// document.querySelectorAll('.remove')[0].click();
console.log(rootElement.innerHTML);
```


### 6. Todo List
```
const TodoItem = (props) => <li onClick={props.onClick}>{props.item.text}</li>

class TodoList extends React.Component {
  render() {
    const { items, onListClick } = this.props;
    return (<ul onClick={onListClick}>
      {items.map((item, index) => 
                 <TodoItem item={item} key={index} onClick={this.handleItemClick.bind(this, item)}/>)}
    </ul>);
  }
  
  handleItemClick(item, event) {
    if(item.done) {
    	event.stopPropagation();
    } else {
    	this.props.onItemClick(item, event);
	}
  }
}


const items = [ { text: 'Buy grocery', done: true },
  { text: 'Play guitar', done: false },
  { text: 'Romantic dinner', done: false }
];

const App = (props) => <TodoList
  items={props.items}
  onListClick={(event) => console.log("List clicked!")}
  onItemClick={(item, event) => { console.log(item, event) }}
/>;

document.body.innerHTML = "<div id='root'></div>";
const rootElement = document.getElementById("root");
ReactDOM.render(<App items={items}/>, rootElement);
```
