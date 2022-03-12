### JSON Setup

##### STEP#1: Install JSON Server:
```
npm install -g json-server
```

##### STEP#2: Prepare db.json file example:

```
{
  "person": [
    {
      "id": 1,
      "personName": "rahul",
      "emailAddress": "vaish.rahul4810@gmail.com"
    },
    {
      "id": 2,
      "personName": "a",
      "emailAddress": "a@gmail.com"
    },
    {
      "id": 3
      "personName": "b",
      "emailAddress": "b@gmail.com",
    },
    {
      "id": 4
      "personName": "c",
      "emailAddress": "c@gmail.com",
    }
  ]
}
```



##### STEP#3: Start JSON Server:
```
json-server --watch db.json
```
