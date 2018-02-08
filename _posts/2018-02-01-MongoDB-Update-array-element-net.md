---
layout: post
title: MongoDB - update array element in C#
subtitle: How to use positional operator using MongoDB C# driver. 
comments: true
tags: MongoDB, C#, DotNet, Positional, Operator
---
*Recently I was trying to perform update of subdocument, within document array, I couldn't find any interesting guidance and that is why I present following solution.*

#### MongoDB document model:

```json
{
    "title": "Presidential elections",
    "author": "Joe Black",
    "description": "Very important poll",
    "votes": [
        {
            "name": "Margaret",
            "yes": true,
            "guid": "6c909a36-8a3d-4b42-926f-85d7c16081b6"
        },
        {
            "name": "Michael",
            "yes": false,
            "guid": "3e226bb0-d39b-4d08-bea2-a707606db7de"
        }
    ]
}
```
<em><BR></em>
#### C# Model:
##### Poll.cs

```csharp
public class Poll : MongoDocument
{
    public string Author { get; set; }
    public string Description { get; set; }
    public PollType PollType { get; set; }
    public string Title { get; set; }
    public List<Vote> Votes { get; set; }
}
```

##### Vote.cs
```csharp
public class Vote
{
    public string Name { get; set; }
    public bool Yes { get; set; }
    public DateTime LastModified { get; private set; }
    public Guid Guid { get; private set; }
}
```

*Let's imagine that we would like to perform update on single vote, using it's GUID.*



We can do that as follows:
```csharp
public async Task<bool> UpdateVote(string id, DTO.Vote vote)
{
    var poll = GetPoll(id);
    var update = Builders<Poll>.Update.Combine(
        Builders<Poll>.Update.Set(x => x.Votes[-1].Name, vote.Name),
        Builders<Poll>.Update.Set(x => x.Votes[-1].Yes, vote.Yes));

    var filter = Builders<Poll>.Filter.And(
        Builders<Poll>.Filter.Eq(x => x.Id, ObjectId.Parse(id)),
        Builders<Poll>.Filter.ElemMatch(x => x.Votes, x => x.Guid == vote.Guid));

    var result = await _context.Polls.UpdateOneAsync(filter, update);
    return result.IsAcknowledged
        && result.ModifiedCount > 0;
}
```