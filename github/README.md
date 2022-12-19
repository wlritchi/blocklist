Users blocked on GitHub for various reasons.

Usually, blocks occur because of a specific comment or pattern of comments by a user, and
accordingly it's easy to link to the source, and easy to identify the user to be blocked, as that
user is the one making the comment(s). However, in some cases, reactions to a comment are
sufficient to warrant a block, such as positive reactions to an obviously unacceptable (usually
bigoted) comment, or negative reactions to a polite pronoun correction (sexism/transphobia).

Querying for reactions to a comment can be accomplished with the following GraphQL query:

```graphql
query { 
  repository(name: "repo-name", owner: "repo-owner") {
    issue (number: 12345) {
      comments (first:7) {
        edges {
          node {
            id,1
            reactions(first:50, content:THUMBS_DOWN) {
              edges {
                node {
                  user {
                    login
                  }
                }
              }
            }
          }
        }
      }
    }
  }
}
```

I don't know what I'm doing with GraphQL at all, so there's probably a much better way to express
that query, especially to select *exactly* the 7th comment, rather than just the first 7. Ideally
there would also be a way to select a comment by its global ID (as seen in e.g. the permalink) but
I didn't see how to do that at a cursory glance.
