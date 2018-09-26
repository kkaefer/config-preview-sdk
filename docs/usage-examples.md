# Providing usage examples of orbs
_The `examples` stanza is available in configuration version 2.1 and later_

As an author of an orb, you may want to document examples of using it in a CircleCI config file, not only to provide a starting point for new users, but also to demonstrate more complicated use cases.

## Simple examples
Given the following orb:

```yaml
version: 2.1
description: A foo orb

commands:
  hello:
    description: Greet the user politely
    parameters:
      username:
        type: string
        description: A name of the user to greet
    steps:
      - run: "echo Hello << parameters.username >>"
```

...you can supply an additional `examples` stanza like this:

```yaml
examples:
  - description: Greeting a user named Anna
    usage:
      version: 2.1
      orbs:
        foo: bar/foo@1.2.3
      jobs:
        build:
          machine: true
          steps:
            - foo/hello:
                username: "Anna"
```

Please note that `examples` is an array, so it can contain multiple entries.

## Expected usage results

The above usage example can be supplemented with a `result` key, demonstrating what the configuration will look like after expanding the orb with its parameters:

```yaml
examples:
  - description: Greeting a user named Anna
    usage:
      version: 2.1
      orbs:
        foo: bar/foo@1.2.3
      jobs:
        build:
          machine: true
          steps:
            - foo/hello:
                username: "Anna"
    result:
      version: 2.1
      jobs:
        build:
          machine: true
          steps:
          - run:
              command: echo Hello Anna
      workflows:
        version: 2
        workflow:
          jobs:
          - build
```

When validating orb's source, the `result` map will be automatically compared with the actual output of compiling `usage`, and an error will be raised in case of a mismatch.

## Example usage syntax
An example usage map can have the following keys:

- **description:** (optional) A string that explains the example's purpose, making it easier for users to understand it.
- **usage:** (required) A full, valid config map that includes an example of using the orb.
- **result:** (optional) A full, valid config map demonstrating the result of expanding the orb with supplied parameters.
