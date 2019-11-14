---
type:               "post"
title:              "Offusquez vos id dans vos url"
date:               "2019-11-06"
publishdate:        "2019-11-06"
draft:              false
slug:               "offusquez-vos-id-dans-vos-url"
description:        "Découverte d'alternatives aux ID auto-incrémentés dans les urls et leur mise en place dans le framework Symfony."

thumbnail:          "/images/posts/thumbnails/obfuscation.jpg"
header_img:         "/images/posts/headers/obfuscation.jpg"
tags:               ["Securite","PHP","Symfony","Framework"]
categories:         ["Dev", "Symfony"]

author_username:    "mcolin"
---

GraphQL est un standard qui s'impose peu à peu dans le monde des API. Comme tout comme tout protocole API, il vient avec différent client facilitant le dialogue avec les API l'utilisant.

[Apollo GraphQL](https://www.apollographql.com/) est une solution SAAS offrant proposant une solution serveur GraphQL et ce qui va nous intéressé aujourd'hui, un très bon [client GraphQL Javascript](https://github.com/apollographql/apollo-client). Le client est open source. Sa force est de proposer un cache applicatif très performant.

## Premiere requête

L'utilisation basique du client est très simple et dans la veine de ce qu'on retrouve dans n'importe quel client API.

L'installation :

{{< highlight bash >}}
# installing the preset package
npm install apollo-boost graphql-tag graphql --save
# installing each piece independently
npm install apollo-client apollo-cache-inmemory apollo-link-http graphql-tag graphql --save
{{< /highlight >}}

On instancie le client avec l'url du endpoint GraphQL :

{{< highlight javascript >}}
import ApolloClient from 'apollo-boost';

const client = new ApolloClient({
  uri: 'https://graphql.example.com'
});
{{< /highlight >}}

Et c'est partie on peut faire nos requête API :

{{< highlight javascript >}}
import gql from 'graphql-tag';

client.query({
  query: gql`
    query TodoApp {
      todos {
        id
        text
        completed
      }
    }
  `,
})
  .then(data => console.log(data))
  .catch(error => console.error(error));
{{< /highlight >}}

A partir de ce moment, cette requête est cachée en mémoire. Vous pouvez la refaire autant de fois que vous le souhaité, aucun appel réseau ne sera fait.

## Mutations

Le client permet évidemment d'exécuter des mutations :

{{< highlight javascript >}}
import gql from 'graphql-tag';

client.mutate({
  mutation: gql`
    mutation AddTodo($text: String!) {
      addTodo(text: $text) {
        id
        completed
      }
    }
  `,
})
  .then(data => console.log(data))
  .catch(error => console.error(error));
{{< /highlight >}}

Grâce a cette mutation, nos données on été mise à jour sur notre serveur. Par contre par la même notre cache n'est plus à jour, et comme dit précédement, Apollo ne refera pas l'appel à l'API.

> Il y a seulement 2 problèmes compliqués en informatique : nommer les choses, et l’invalidation de cache.

Nous allons donc devoir mettre à jour ce cache.

## Lire et écrire dans le cache

Le cache d'Apollo et son paradygme est un peu spécial. Il ne s'agit pas seulement d'un simple cache de requête. Il agit un peu comme un *state manager*. Les données récupéré via une requête sont automatiquement stocker dans le cache mais il nous est possible d'y accéder et même d'y inserer ou d'y modifier de données sans refaire de requête.




## Configurer le cache

https://www.apollographql.com/docs/react/caching/cache-configuration/
