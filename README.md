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
### 5. Image Gallery App
### 6. Todo List
