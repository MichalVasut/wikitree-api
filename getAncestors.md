# WikiTree API: getAncestors

The getAncestors action returns one or more person profiles like those from [getProfile](getProfile.md). From a starting profile, specified by a "key" that is a WikiTree ID or User ID, the mother/father values are followed to a requested depth.

## Parameters

|Param|Value|
|-----|-----|
|action|getAncestors|
|key|WikiTree ID or Id|
|depth|The number of generations back to follow the parent ids|
|fields|Optional comma-separated list of fields to return for each profile|
|bioFormat|Optional: "wiki", "html", or "both"|
|resolveRedirect|Optional. If 1, then requested profiles that are redirections are followed to the final profile|

### key

The "key" parameter is used to indicate which profile to return. This can be either a "WikiTree ID" or a "User ID". The WikiTree ID is the name used after "/wiki" in the URL of the Person page. For example, for [Person Profile Pages](https://www.wikitree.com/wiki/Help:Person_Profile) like https://www.wikitree.com/wiki/Shoshone-1, the WikiTree ID is "Shonshone-1".

Note that the Mother and Father fields in a Person profile are (User) IDs. Depending on what data you retrieve for Mother/Father it may be easier to use getPerson to follow those than getProfile (which requires a Page ID or WikiTree ID)

### depth

The depth is how many steps/generations to recursively follow the mother/father ids on each profile to gather ancestors. A depth of 1 returns the parents, 2 returns parents & grandparents, etc.

### fields

The "fields" parameter is optional. If left out, a default set of fields is returned. For Person profile pages, the default is all fields other than the biography, children and spouses. For Free-Space profile pages, the default is to return all fields.

You can specify which fields to return by setting the "fields" parameter to a comma-separated list of those you want. You can also use "*" to indicate "all fields". 

### bioFormat

If you request the "bio" field (the text biography for a Person profile), the default is to return the content as it's stored, with wiki markup. You can instead request that this markup be rendered into HTML (as it would appear on the profile's web page) by specifying a "bioFormat" of "html". If you use a bioFormat value of "both", then both the original wiki text and the rendered HTML will be returned.

### resolveRedirect

Generally if you start at a valid profile and follow the ids associated with relationships (mother, father) you should get a valid/complete profile in return. However, in some circumstances you may end up requesting a profile that has been merged away into another profile, or otherwise is redirected. If you set resolveRedirect=1 in your action, then any profiles that would be returned that are redirections will be followed to their end point, and *that* final profile will be returned.

## Results

|Field|Description|
|-----|-----------|
|ancestors|Array of Person profile data (with fields specified by the "fields" parameter). 

See [getProfile.md](getProfile.md) for the fields in each Person profile.


## Examples

```
curl 'https://api.wikitree.com/api.php?action=getAncestors&key=Adams-35&depth=1&fields=Id,Name,Mother,Father'


[
  {
    "user_id": "3636",
    "ancestors": [
      {
        "Id": 3636,
        "Name": "Adams-35",
        "Father": 3640,
        "Mother": 3641
      },
      {
        "Id": 3640,
        "Name": "Adams-38",
        "Father": 3642,
        "Mother": 3643
      },
      {
        "Id": 3641,
        "Name": "Bass-1",
        "Father": 3660,
        "Mother": 3661
      }
    ],
    "status": 0
  }
]
```

* [JavaScript](examples/getPerson/javascript.html)
