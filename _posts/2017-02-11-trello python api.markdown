---
title: Using the Trello Python API
date: 2017-02-11 15:39:00 +05:30
tags:
- python
- api
- trello
layout: post
image: assets/images/trello.png
---

This is a quick tutorial on using the [Trello Python API](https://pypi.python.org/pypi/trello).

Trello is a great tool that I use to keep myself updated with various tasks and ToDo's. You can have a look at the API provided by Trello [here](https://developers.trello.com/advanced-reference).

* First get your Trello API key and access token from [here](https://trello.com/app-key).

* Once you have the access token and API key create a client:

```python 
from trello import TrelloApi
client = TrelloApi(your_api_key, your_token)
```

* Now you can get a list of all Boards by using the code below:

```python 
client.members.get_board('me')
```

* To get a list of lists in a board:

```python 
client.boards.get_list(board_id)
```

* To get all cards in a list:

```python 
client.lists.get_card(list_id)
```

THE END.
