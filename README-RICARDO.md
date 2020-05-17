# Tutoriales

1 - https://www.youtube.com/watch?v=fY3mBJSDA44 (Downloading started demo & create a contentful space and post)
2 - https://www.youtube.com/watch?v=IaNU4R3ck_k (Setting puglins, environment and TOKEN)
3 - https://www.youtube.com/watch?v=L9Uv_bLSaP4 (Create queries)
4 - https://www.youtube.com/watch?v=NkFz2psDupw (Create post template from gatsby-node.js)
5 - https://www.youtube.com/watch?v=A8Y1-GRmxFw (Post template & Author data)
6 - https://www.youtube.com/watch?v=ONg1gpx0zlA (Image in posts)

## Create a space

1. Create a contenful account (github login)
2. Create a space
3. Create new content type

## Config plugins

```
npm install --save gatsby-source-contentful --save
```

```
{
  resolve: `gatsby-source-contentful`,
  options: {
    spaceId: `p1zrjq7oulxr`,
    accessToken: process.env.CONTENTFUL_ACCESS_TOKEN,
  },
},
```

```
npm install dotenv --save
```

## Setting graphQL query

```
{
  allContentfulWork {
    edges {
      node {
        title
        author
        slug
        image {
          fluid {
            src
          }
        }
        content {
          childContentfulRichText {
            html
          }
        }
      }
    }
  }
}
```

## Richtext plugin

```
 npm i @contentful/gatsby-transformer-contentful-richtext --save
 plugins: [`@contentful/gatsby-transformer-contentful-richtext`]
```

```
content {
  childContentfulRichText {
    html
  }
}
```

## Setting gatsby-node.js

```
const path = require(`path`)

exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions

  const blogPost = path.resolve(`./src/templates/blog-post-contentful.js`)
  const result = await graphql(
    `
    {
      allContentfulWork {
        edges {
          node {
            title
            author
            slug
            image {
              fluid {
                src
              }
            }
            content {
              childContentfulRichText {
                html
              }
            }
          }
        }
      }
    }
    `
  )

  if (result.errors) {
    throw result.errors
  }

  // Create blog posts pages.
  const posts = result.data.allContentfulWork.edges

  posts.forEach((post, index) => {
    const previous = index === posts.length - 1 ? null : posts[index + 1].node
    const next = index === 0 ? null : posts[index - 1].node

    createPage({
      path: post.node.slug,
      component: blogPost,
      context: {
        slug: post.node.slug,
        previous,
        next,
      },
    })
  })
}
```

## Setting image

```
...
contentfulWork( slug: { eq: $slug }) {
      title
      author
      content {
        childContentfulRichText {
          html
        }
      }
      image {
        fluid {
          ...GatsbyContentfulFluid
        }
      }
    }
...
```

## Setting styled-components

```
npm install styled-components --save
```

Example:

```
const PostImage = styled.div`
  flex: 25%;
  margin-right: 1rem;
`

<PostImage>
  <Img fluid={ node.image.fluid } />
</PostImage>
```



