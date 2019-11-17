Absinthe 1.5 is going to be released soon after the conference.
  1. Graphql SDL support



GraphQL Basics:
  1. GraphQL 
    * Graph Query Language
    * What you ask for is what you get back
  2. Most of the time, people compare it to a HTTP API (REST)
  3. Posting JSON and getting JSON back
  4. Query
    * Requests Information
  5. Mutation
    * Modifies data, requests information in response
  6. Subscription
    * Notifies when a specific thing happens
    * Websockets could be ideal

  7. Behind the queries, there is the SCHEMA!!! The schema is the graph itself. 
    * Nodes are the things in the schema
    * Edges are how somethign is related to something else
      1. Post -> User -> Post (How it is related)
      2. Edges are also the types for the things that you want for a field. (Datetime, etc)
  8. You cant start just anywhere. 3 main operation types (you have to start at Query, mutation, subscription)
  9. Resolvers sit on every field (the code to make things work)
  10. Think Russian Nesting Dolls. You can drill down in at each layer for more and more
  
  11. Introspection (LEARN MORE ABOUT THIS)
    * 
  12. Tracing
    * Allows us to see what is actually being used in the queries and thus we can deprecate things if needed.
    * We need to add APOLLO TRACING!!!!

  https://github.com/bruce/pastex - App we will be working on
  slides.com/wbruce/production-ready-apis
    creatuser --superuser postgres will create a postgres user



They recommend scoping permissions through structurally 
By only having the single point (me/current_user) you can try and limit auth checks
By limiting how things can be retrieved, it limits things automatically.
For permissions, they recommend trying not to do any database calls for permissions at all if possible.


It is okay to return errors and data at the same time
IdentityTypes.ex line 53 



Middleware Looks AMAZING!!!!! Look into it for our auths.
Can pattermatch on the middleware in the schema to scope it tighter.
  3rd field, object, %{identifier: :user}
  

COMPLEXITY ANALYSIS!!
In the router file for graphiql:
  analyze_complexity: true,
  max_complexity: 1_000



you can return a keyword list to graphql if you want, not just an {:error, ""}
error = [
  message: "unauthorized",
  code: 403
]

create paste
{
  "pasteInput": {
    "name": "I made this",
    "description": "In the middle of class even",
    "files" : [
      {"name": "kaboom.ex", "body": "raise \"kaboom\""}
    ]
  }
}



{
  pastes(first:10) {
    results: edges {
      cursor
      paste: node {
        name
        author {
          name
        }
      }
    }
    pageInfo {
      hasNextPage
      startCursor
      endCursor
    }
  }
}


# Try to write your query here
{
  health
}


query IntrospectionQuery {
    __schema {
      queryType { name }
      mutationType { name }
      subscriptionType { name }
      types {
        ...FullType
      }
      directives {
        name
        description
        locations
        args {
          ...InputValue
        }
      }
    }
  }

  fragment FullType on __Type {
    kind
    name
    description
    fields(includeDeprecated: true) {
      name
      description
      args {
        ...InputValue
      }
      type {
        ...TypeRef
      }
      isDeprecated
      deprecationReason
    }
    inputFields {
      ...InputValue
    }
    interfaces {
      ...TypeRef
    }
    enumValues(includeDeprecated: true) {
      name
      description
      isDeprecated
      deprecationReason
    }
    possibleTypes {
      ...TypeRef
    }
  }

  fragment InputValue on __InputValue {
    name
    description
    type { ...TypeRef }
    defaultValue
  }

  fragment TypeRef on __Type {
    kind
    name
    ofType {
      kind
      name
      ofType {
        kind
        name
        ofType {
          kind
          name
          ofType {
            kind
            name
            ofType {
              kind
              name
              ofType {
                kind
                name
                ofType {
                  kind
                  name
                }
              }
            }
          }
        }
      }
    }
  }




{
  heaath
}


{
  explode
  health
}


{
  pastes {
    name
    files {
      name
      paste {
        name
        files {
          name
          paste {
            name
          }
        }
      }
    }
  }
  health
}



{
  pastes(first:89) {
    results: edges {
      cursor
      paste: node {
        name
        author {
          name
        }
      }
    }
  }
}


subscription {
  pasteCreated {
    id
    name
  }
}





mutation ($pasteInput:CreatePaste) {
  createPaste(input:$pasteInput) {
    id
    files {
      name
    }
  }
  
}
{
  "pasteInput": {
    "name": "I made this",
    "description": "In the middle of class even",
    "files" : [
      {"name": "kaboom.ex", "body": "raise \"kaboom\""}
    ]
  }
}




mutation {
  createSession(
    email:"rich@localhost.com",
    password:"abc123"
  ) {
    token
  }
}
Authorization   Bearer SFMyNTY.g3QAAAACZAAEZGF0YWECZAAGc2lnbmVkbgYAtjqKqmUB.8jrsEMqPRoi9hE-U9N252aQ1y4h5dL8v04vcLbBHciY



{
  me {
    name
  }
}











Class 1 - Debugging
IO.warn is cool, uses red and gives a stack trace
Iex Pry has cool potential.
  It only stops the one single process, all other processes continue

Clas 2 - Flow
  Use a Step Module to determine and verify your pipeline
  THe Token approach is good for very well defined requirements
  Archetecting Flow blog series
    trivelop.de website


Class 3 - Growing Applications
  "If you cant use your app from IEX, then you need to restructure your apis"

  We want things to go through apis so we can control what power is given people

  Make sure that you are organzing your @docs! 
  Public API == Contract with outside world
    Limit docs to public api
  Mox Library???
  Hexagonal Architechure. 
  Move validation to edges to stop bad data from having to travel though app
  


  Class 4 - Sustainable Testing
    Using propery testing
    
    Write whatever test that has the highest probability of finding a bug now or in the future. Concentrate more on the critical parts.


  Class 5
    Mix deps.compile --warnings-as-errors

    Using Distillery to use releases to help find things as well

    mix release / mix release --name bellevue
    ./bin bellevue remote_console

    This guy is showing CircleCI


Class 6 - Erlang/OTP What is in the box?
  Erlang and Elixir are really just one in the same
  Modules are Atoms


