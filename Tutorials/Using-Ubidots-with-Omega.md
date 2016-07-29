# Using Ubidots with the Omega

It is possible to get the Omega to write to variables on Ubidots through the console. 

## Using the ubidots-client

To allow the Omega to communicate with Ubidots, a package must be installed via opkg from the console of the Omega. The command to enter is:

```
opkg install ubidots-client
```

Now whenever you want to write to a variable in ubidots, send a command through the command line with your token, data source label, the variable, and the value you wish to assign to it.

The flags -t (token) and -d (data source label) precede the respective values, and 'set' will precede the json string containing the variable and new value to assign. Multiple variables, with multiple values, may be assigned at the same time.

For example:

```
ubidots -t <my-token> -d <my-data-source-label> set '{"variableOne":12, "variableTwo":10}'
```

To get the most recent value of a variable from ubidots, change 'set' to 'get' and the following json file to the variable name of interest. Only one variable can be fetched at once.

For example:

```
ubidots -t <my-token> -d <my-data-source-label> get variableOne
```