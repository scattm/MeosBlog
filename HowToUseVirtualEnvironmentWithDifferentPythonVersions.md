Once upon a time, I needed to create 2 different Virtual Environment for testing my Python Application. So I note it down here, simple but cool trick.

- Firstly, figure out where is your Python binary on your system
- Secondly, create the Virtual Environment

Here is the example:

    $ which python3
    /usr/local/bin/python3
    $ virtualenv -p /usr/local/bin/python3 ~/example_env

Voila, now you can use your new created Virtual Environment.