The cli tool is a Work In Progress. The npm package exists to reserve the name! Look for progress at https://github.com/karuppiah7890/cayaml 😄 🎉

The idea of this project is to store command(s) as a yaml file.

The idea is an inspiration from the concept of templates and values in [Helm](https://github.com/helm/helm) - a package manager for Kubernetes.

For example, let's say you write a shell script which does a lot of curl calls with small differences in arguments / flags, then you can store these commands as yamls and maintain them in a better and readable manner. Here's an example :

Let's say you want to execute this command with different data each time:

```
curl -H "Content-Type: application/json" -X POST -d '{"user":"bob","pass":"123"}' http://example.com
```

You can store the command as a yaml template like this:

```
$ cat curl.template.yaml

name: curl
flags:
    - name: -H
      value: 'Content-Type: {{ dataType }}'
    - name: -X
      value: {{ method }}
    - name: -d
      value: {{ data }}
arguments:
    - {{ url }}
```

and then the values like this:

```
$ cat curl.values.yaml

method: POST
dataType: application/json
data: {
  "user": "bob",
  "pass": "123"
}
url: http://example.com
```

and when applying the values to the template, you finally get a yaml like this:

```
$ cat curl.yaml

name: curl
flags:
    - name: -H
      value: 'Content-Type: application/json'
    - name: -X
      value: POST
    - name: -d
      value: {
        "user": "bob",
        "pass": "123"
      }
arguments:
    - http://example.com
```

And with different values yaml, you will get different final yaml.

My idea is to build a tool to execute a command which is represented as a yaml, and to also help in creating dynamic commands as yamls from templates and values and then run them too 🎉
