# Azure-Logic-Apps-Workflows
This repo is to show how we can use Azure Logic App (Standard) to build multiple workflows and chain them together.

## Workflow 1
The Http trigger looks for a body of
~~~
{
  "email":"abcd@abcd.com"
}
~~~
Then a new variable user_email is set up.

If the user_email is empty, an error message is returned:
~~~
{
    "error": "email is empty"
}
~~~

Otherwise, a Compose action is called to build a JSON
~~~
{
  "user_emai":"abcd@abcd.com"
}
~~~
The output of the Compose is used to call out for IP addresses from Workflow 2.

The returned body is directly fed to the caller.

## Workflow 2
Based on if the value of user_email is ended with ".com" or not, two Response actions return different IP addresses. For example, if Http request with body of
~~~
{"email":"abcd@abcd.com"}
~~~
The response of whole workflows is
~~~
{
    "ips": "10.10.10.10, 10.10.10.11"
}
~~~
If the Http reuqest body changes to 
~~~
{"email":"abcd@abcd.edu"}
~~~
The response is
~~~
{
    "ips": "100.10.10.10, 100.10.10.11"
}
~~~

