<emp># Simple-GraphQL
Shows query potential of GraphQL.

Courtesy of Web Dev Simplified on YouTube </emp>

const express = require('express')
const expressGraphQL = require('express-graphql')
const {
    GraphQLSchema,
    GraphQLObjectType,
    GraphQLString,
    GraphQLList,
    GraphQLInt,
    GraphQLNonNull
} = require('graphql')
const app = express()

const authors = [
    { id: 1, name: 'J.K. Rowling' },
    { id: 2, name: 'J. R. R. Tolkien' },
    { id: 3, name: 'Brent Weeks' }
]

const books = [
    { id: 1, name: 'Harry Potter and the Chamber of Secrets', authorID: 1 },
    { id: 2, name: 'Harry Potter and the Prisoner of Azkhaban', authorID: 1 },
    { id: 3, name: 'Harry Potter and the Goblet of Fire', authorID: 1 },
    { id: 4, name: 'The Fellowship of the Ring', authorID: 2 },
    { id: 5, name: 'The Two Towers', authorID: 2 },
    { id: 6, name: 'The Return of the King', authorID: 2 },
    { id: 7, name: 'The Way of Shadows', authorID: 3 },
    { id: 8, name: 'Beyond the Shadows', authorID: 3 },
]

const BookType = new GraphQLObjectType({
    name: 'Book',
    description: 'This represents a book written by an author',
    fields: () => ({
        id: { type: GraphQLNonNull(GraphQLInt) },
        name: { type: GraphQLNonNull(GraphQLString) },
        authorID: { type: GraphQLNonNull(GraphQLInt) },
        author: {
            type: AuthorType,
            resolve: (book) => {
                return authors.find(author => author.id === book.authorID)
            }
        }
    })
})

const AuthorType = new GraphQLObjectType({
    name: 'Author',
    description: 'This represents an author of book',
    fields: () => ({
        id: { type: GraphQLNonNull(GraphQLInt) },
        name: { type: GraphQLNonNull(GraphQLString) },
    })
})

const RootQueryType = new GraphQLObjectType({
    name: 'Query',
    description: 'Rooty Query',
    fields: () => ({
        books: {
            type: new GraphQLList(BookType),
            description: 'List of All Books',
            resolve: () => books
        }
    })
})

const schema = new GraphQLSchema({
    query: RootQueryType
})



app.use('/graphql', expressGraphQL({
    schema: schema,
    graphiql: true
}))
app.listen(5000., () => console.log('Server Running'))

