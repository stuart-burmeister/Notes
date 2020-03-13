# Style Rules

## File structure
* Each component should have it own file and directory
* The directory and file names should be the same as the component name
* Sub components are also managed as above (check for examples)
* All components should have an index.js file exporting the component
```javascript
// Desk/index.js
export { default } from "./Desk";
```
* All the components should also be exported from the components/index.js file as well.
```javascript
// Desk/components/index.js
export { default as InputField } from "./InputField";
export { default as MsgList, LIST_MSG } from "./MsgList";
export { default as UserList, SEARCH_USER } from "./UserList";
```

## Props & Creating components
* All components start with props
* Decompose the props into local variables for use (ie no props.variable)
```javascript
 const WarnDialog = props => {
   const { open, onClose, onAgree, onDissagree } = props;
   ...
 }
```
* Do not specify props if none are used.
```javascript
const Desk = () => {
 ...
}
```

## Exporting
* Export at the end of the file, not at the function definition.
```javascript
import React from "react";

const Desk = () =>{
 ...
}

export default MsgList;
export { LIST_MSG };
```

## Dialog
* Implement separate from the components
* Use callbacks rather than handlers for passing functionality down.
```javascript
// Callbacks ( O )
<WarnDialog
 open={warn.status}
 onClose={() => {
   setWarn({ status: false });
 }}
 onAgree={() => {
   removeMessage({ variables: { id: warn.value.id } });
   setWarn({ status: false });
 }}
 onDissagree={() => {
   setWarn({ status: false });
 }}
/>
```
```javascript
// Passing Handler ( X )
const Test = () => {
 const [open, setOpen] = setState("init")

 <WarnDialog
   open={value}
   setOpen={setOpen}
 />
}

const WarnDialog = (props) => {
 const {open, setOpen } = props
 ...
 <Button onClick={()=>{setOpen(false)}}>닫기</Button>
 ...
}
```
* Use the basic structure of Title, Content, then Action.
```javascript
const WarnDialog = props => {
  const { open, onClose, onAgree, onDissagree } = props;

  return (
    <Dialog open={open} onClose={onClose}>
      <DialogTitle>Warn!</DialogTitle>
      <DialogContent>
        Are you sure delete? No rollback if item delete.
      </DialogContent>
      <DialogActions>
        <Button
          variant="contained"
          onClick={() => {
            onAgree && onAgree();
          }}
        >
          ok
        </Button>
        <Button
          variant="contained"
          onClick={() => {
            onDissagree && onDissagree();
          }}
        >
          cancel
        </Button>
      </DialogActions>
    </Dialog>
  );
};
```

## Layout/Screen Drawing
* Use Box and grid for layouts.
* When applicable, start the component with Box
* Use the Grid container and Grid item to divide the screen repeatedly.
```javascript
const Desk = () => {
  return (
    <Box>
      <Grid container>
        <Grid item>
          <Grid container>
            <Grid item>
              <InputField />
            </Grid>
            <Grid item>
              <UserList />
            </Grid>
          </Grid>
        </Grid>
        <Grid item>
          <MsgList />
        </Grid>
      </Grid>
    </Box>
  );
};
```

## CSS
* Use material-ui styles as well as Hook API
* Do not use inline styling.
* For merging styles use clsx
* The first element of the component should be labelled as the 'root'
* Commonly used style (for merging) is written above the route.
* Declare any variables used in styling directly above useStyles.
* Name each style in the same order at the code, and with combinations of '__' (2 underlines) and words (lowercase)
```javascript
import { Box, Grid } from "@material-ui/core";
import { makeStyles } from "@material-ui/styles";
import clsx from "clsx";
import React from "react";
import { InputField, MsgList, UserList } from "./components";

const fixedHeight = 300;
const useStyles = makeStyles(() => ({
  height: { height: "100%" },

  root: {
    width: "100%"
  },
  container: { flexWrap: "nowrap" },
  item__inputfield__userlist: {
    minWidth: 600,
    width: "50%",
    height: "100%"
  },
  container__inputfield__userlist: { flexDirection: "column" },
  item__inputfield: { width: "100%", height: fixedHeight, padding: 8 },
  item__userlist: {
    width: "100%",
    height: `calc(100% - ${fixedHeight}px)`,
    padding: 8
  },
  item__msglist: { minWidth: "400px", width: "50%", padding: 8 }
}));

const Desk = () => {
  const classes = useStyles();

  return (
    <Box className={clsx(classes.root, classes.height)}>
      <Grid container className={clsx(classes.container, classes.height)}>
        <Grid item className={classes.item__inputfield__userlist}>
          <Grid
            container
            className={clsx(
              classes.container__inputfield__userlist,
              classes.height
            )}
          >
            <Grid item className={classes.item__inputfield}>
              <InputField />
            </Grid>
            <Grid item className={classes.item__userlist}>
              <UserList />
            </Grid>
          </Grid>
        </Grid>
        <Grid item className={clsx(classes.item__msglist, classes.height)}>
          <MsgList />
        </Grid>
      </Grid>
    </Box>
  );
};

export default Desk;
```

## Common Loading Screen
* Use an overlay for the component, instead of as a separate dialog.
* Use the CCLoading component as default.
* In order to use the loading screen, a promise is needed.
* The overlay should be implemented over the entire component, as follow:
```javascript
const useStyles = makeStyles(theme => ({
  root: {
    height: "100%",
    width: "100%",
    position: "absolute",
    top: 0,
    left: 0,
    backgroundColor: "gray",
    opacity: 0.9
  }
}));
```
* When using the above style, you need to specify the position relative to the component you want to use.
```javascript
const useStyles = makeStyles(theme => ({
  root: {
    height: "100%",
    width: "100%",
    position: "relative", // for Loading component
    overflow: "auto",
    border: "1px solid",
    boxSizing: "border-box",
    padding: 8
  }
}));

const MsgList = () => {
  return (
    <Box className={classes.root}>
      ...
      <Loading open={loading || removeMessageLoading} msg={"Loading..."} />
      ...
    </Box>
  );
};
```

## GraphQL
* Use Graphql with the Apollo client.
* Use the method examples given below for using the Apollo Client.
* By default, react hook methods useQuery and useMutation are used.

### useQuery
* If data does not need to be proccessed, then simply getting data from the query call is enough.
```javascript
const UserList = () => {
  ...
  const { loading, data } = useQuery(SEARCH_USER, {
    onCompleted: data => {},
    onError: err => {
      alert(err);
    },
    fetchPolicy: "network-only"
  });

  return (
    <Box>
      <Loading open={loading || removeUserLoading} msg={"Loading..."} />
      <Box>
        <Typography variant="h4">User List</Typography>
      </Box>
      {!loading &&
        data &&
        data.searchUser.map((user, index) => {
          ...
          );
        })}
      ...
    </Box>
  );
};
```
* If data needs to be processed, use an event hook to unload the data.
```javascript
const MsgList = () => {
  ...
  const [contents, setContents] = useState([]);
  const { loading, data } = useQuery(LIST_MSG, {
    fetchPolicy: "network-only",
  });

  useEffect(() => {
    if (data && data.messages) {
      setContents(data.messages);
    }
  }, [data]);

  return (
    <Box>
      ...
      {!loading &&
        contents.map((msg, index) => {
          ...
        })}
      ...
    </Box>
  );
};
```
* If data needs to be polled, use a 5s interval.
```javascript
const LIST_MSG = gql`
  query messages {
    messages {
      id
      text
    }
  }
`;

const MsgList = () => {
    /* 5s */
  const { loading, data } = useQuery(LIST_MSG, {
    fetchPolicy: "network-only",
    pollInterval: 5000
  });
```

### useMutation
* When used by itself, simply use the useMutation call and call the function with variables.
```javascript
// Data is processed in onCompleted and onError as below.
/* Declaration */
const [createMsg, { loading: msgLoading }] = useMutation(CREATE_MSG, {
  onCompleted: data => {},
  onError: err => {
    alert(err);
  }
});
```
```javascript
/* Usage */
createMsg({
  variables: {
    user: emailRef.current.value,
    text: textRef.current.value
  }
});
```
* When another mutation is called after another, place the second in the onCompleted callback function.
```javascript
// When calling multiple mutations, place the function call of the second mutation in the onCompleted of the first mutation to be called, as shown below.
/* Declaration */
const [createMsg, { loading: msgLoading }] = useMutation(CREATE_MSG, {
  onCompleted: data => {},
  onError: err => {
    alert(err);
  }
});
const [signUpNcreateMsg, { loading: signupLoading }] = useMutation(SIGNUP, {
  onCompleted: data => {
    createMsg({
      variables: {
        user: emailRef.current.value,
        text: textRef.current.value
      }
    });
  },
  onError: err => {
    alert(err);
  }
});
```
```javascript
/* Usage */
signUpNcreateMsg({
  variables: {
    email: emailRef.current.value,
    username: nameRef.current.value,
    password: passwordRef.current.value
  }
});
```
* When a query has to be updated, use refetchQueries and ensure the original query call is the same (import) and the variables are the same.
```javascript
// Use refetchQueries and awaitRefetchQueries to maintain data..
/* InputField.js */
import { LIST_MSG } from "views/Desk/components";

const [createMsg, { loading: msgLoading }] = useMutation(CREATE_MSG, {
  onCompleted: data => {},
  onError: err => {
    alert(err);
  },
  refetchQueries: [{ query: LIST_MSG /* , variables: {...} */ }],
  awaitRefetchQueries: true
});
```
```javascript
/* MsgList.js */
const LIST_MSG = gql`
  query messages {
    messages {
      id
      text
    }
  }
`;

const MsgList = () => {
  const { loading, data } = useQuery(LIST_MSG, {
    fetchPolicy: "network-only",
    pollInterval: 5000
  });

export { LIST_MSG };
```

## Example Component
```javascript
// import
import { useMutation, useQuery } from "@apollo/react-hooks";
import { Box, IconButton, Typography } from "@material-ui/core";
import DeleteIcon from "@material-ui/icons/Delete";
import { makeStyles } from "@material-ui/styles";
import gql from "graphql-tag";
import React, { useEffect, useState } from "react";
import { Loading, WarnDialog } from "views/components/";

// graphql
const LIST_MSG = gql`
  query messages {
    messages {
      id
      text
    }
  }
`;

const REMOVE_MSG = gql`
  mutation removeMessage($id: ID!) {
    removeMessage(id: $id)
  }
`;

// css
const useStyles = makeStyles(() => ({
  height: { height: "100%" },
  root: {
    height: "100%",
    width: "100%",
    position: "relative",
    overflow: "auto",
    border: "1px solid",
    boxSizing: "border-box",
    padding: 8
  },
  contents__row: {
    display: "flex",
    alignItems: "center",
    borderBottom: "1px solid"
  }
}));

// component
const MsgList = () => {
  // props
  // classes
  const classes = useStyles();

  // value
  const [warn, setWarn] = useState({ status: false, value: null });
  const [contents, setContents] = useState([{ id: "dumy1", text: "dumy1" }]);

  //graphql
  const [removeMessage, { loading: removeMessageLoading }] = useMutation(
    REMOVE_MSG,
    {
      onCompleted: data => {},
      onError: err => {
        alert(err);
      },
      refetchQueries: [{ query: LIST_MSG }],
      awaitRefetchQueries: true
    }
  );
  const { loading, data } = useQuery(LIST_MSG, {
    fetchPolicy: "network-only",
    pollInterval: 5000
  });

  // function

  // useEffect
  useEffect(() => {
    if (data && data.messages) {
      setContents(data.messages);
    }
  }, [data]);

  return (
    <Box className={classes.root}>
      {/* classes.root로 시작 */}
      <Typography variant="h4">MSG LIST</Typography>
      {contents.map((msg, index) => {
        return (
          <Box key={index} className={classes.contents__row}>
            <IconButton
              aria-label="delete"
              onClick={() => {
                setWarn({ status: true, value: msg });
              }}
            >
              <DeleteIcon />
            </IconButton>

            <Typography variant="body1">
              {msg.id}, {msg.text}
            </Typography>
          </Box>
        );
      })}
      <Loading open={loading || removeMessageLoading} msg={"Loading..."} />
      {/* Dialog */}
      <WarnDialog
        open={warn.status}
        onClose={() => {
          setWarn({ status: false });
        }}
        onAgree={() => {
          removeMessage({ variables: { id: warn.value.id } });
          setWarn({ status: false });
        }}
        onDissagree={() => {
          setWarn({ status: false });
        }}
      />
    </Box>
  );
};

export default MsgList;
export { LIST_MSG };
```

## Console
* Use custom console.log, info, warn, and error console messages.
* You can change the settings to output or not based on type.

## Commit Checklist
* [ ] Are imports organised? (mac: option + shift + o)
* [ ] Have all warnings been resolved?
* [ ] Have all 'console.log's been removed?

## Link
* [Original rules](https://github.com/vtsw/vtstyle/blob/master/src/documents/codes.md)
