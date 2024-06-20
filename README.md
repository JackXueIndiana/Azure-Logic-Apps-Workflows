# Azure-Logic-Apps-Workflows
This repo is to show how we can use Azure Logic App (Standard) to build multiple workflows and chain them together.

## Workflow 1 (wf1.png, wf1.json)
The Http trigger looks for a body of
~~~
{
  "email":"abcd@abcd.com"
}
~~~
Then a new variable user_email is set up with an Variable action.

An ction fo Compare is used to see if the user_email is empty, then an error message is returned with a Response action:
~~~
{
    "error": "email is empty"
}
~~~

Otherwise, a Compose action is called to build a JSON
~~~
{
  "user_email": "@{variables('user_email')}"
}
~~~
The output of the Compose is used to call out for IP addresses from Workflow 2.

The returned body is directly fed to the caller.

## Workflow 2 (wf2.png, wf2.json)
A Http trigger lloks for an request with body of schema
~~~
{
    "type": "object",
    "properties": {
        "user_email": {
            "type": "string"
        }
    }
}
~~~
The action of Compare is used to see if the value of user_email is ended with ".com" or not, and two Response actions return different IP addresses accordingly. 

## Testing
From Postman, Https POST calls are made with the URL from the wf1 http trgger's (system generated at creation time). For example, if a Http request is with body of
~~~
{
  "email":"abcd@abcd.com"
}
~~~
Then the response of whole workflows is
~~~
{
    "ips": "10.10.10.10, 10.10.10.11"
}
~~~
If the Http reuqest body changes to 
~~~
{
  "email":"abcd@abcd.edu"
}
~~~
The response is
~~~
{
    "ips": "100.10.10.10, 100.10.10.11"
}
~~~

