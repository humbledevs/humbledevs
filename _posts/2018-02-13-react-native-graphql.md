---
title: React Native - GraphQL
date: 2018-02-13 15:27:31
---

React Native is great framework for building native mobile apps using React. The concepts of React like Virtual DOM, Functional Components, Unidirectional Data Flow has made possible to write much simplified applications. It has enabled building tools like React Native which allows creating native mobile apps for multiple paltforms using same codebase.

GraphQL also made at Facebook enables writing highly performant api. GraphQL allows client side to ask for just the data it requires. Its very useful for mobile devices which are often on slower network connections.

Apollo Graphql is the most popular open-source platform for writing GraphQL Queries and Graphql Server. Apollo GraphQL can be nicely integrated with React Native applications.

<img style="width: 100px;margin: 30px 40%;" src="/images/mobilecloud.png" />

Here are the steps to configure Apollo Graphql in React Native app. For this app I used [Github GraphQL API](https://developer.github.com/v4/):

1. Install required libraries:

    ```
    npm install react-apollo --save
    npm install apollo-client --save
    npm install graphql-tag --save
    ```

2. Create apollo client

    ```
    import { ApolloClient, createNetworkInterface } from "apollo-client";
    import { ApolloProvider } from "react-apollo";

    const networkInterface = createNetworkInterface({
      uri: "https://api.github.com/graphql"
    });
    networkInterface.use([
      {
        applyMiddleware(req, next) {
          if (!req.options.headers) {
            req.options.headers = {};
          }
          req.options.headers.authorization = `Bearer ${Config.GITHUB_TOKEN}`;
          next();
        }
      }
    ]);

    const client = new ApolloClient({
      networkInterface
    });

    export default (() => (
      <ApolloProvider client={client}>
        <RootComponent />
      </ApolloProvider>
    ));
    ```
3. Add queries to components:

    ```
    import gql from "graphql-tag";
    import { graphql } from "react-apollo";

    const UserDetailsQuery = gql`
      query($login: String!) {
        user(login: $login) {
          avatarUrl
          bio
          name
        }
      }
    `;

    class UserDetails extends React.Component {
      // Component code here
    }

    export default (UserDetailsMapped = graphql(UserDetailsQuery)(UserDetails));

    ```

The code for example above is [here](https://github.com/humbledevs/react-native-graphql). The integration is straightforward and advantages many.