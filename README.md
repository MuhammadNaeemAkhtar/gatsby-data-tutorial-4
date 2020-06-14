# NOTES

## dependencies

Typography theme “Kirkham”, and CSS-in-JS library, “Emotion”

npm install --save gatsby-plugin-typography typography react-typography typography-theme-kirkham gatsby-plugin-emotion @emotion/core

Tutorial Link: https://www.gatsbyjs.org/tutorial/part-four/

1. powerful feature of Gatsby that lets you easily build sites from Markdown, WordPress, headless CMSs, and other data sources of all flavors.
2. Gatsby’s data layer is powered by GraphQL.
3. For the purpose of working in Gatsby, however, a more useful answer is “everything that lives outside a React component” is Data.
4. Data can also live in file types like Markdown, CSV, etc. as well as databases and APIs of all sorts. Gatsby’s data layer lets you pull data from these (and any other source) directly into your components — in the shape and form you want.
5 Do I have to use GraphQL and source plugins to pull data into Gatsby sites? 
Absolutely not! You can use the node createPages API to pull unstructured data into Gatsby pages directly, rather than through the GraphQL data layer. This is a great choice for small sites, while GraphQL and source plugins can help save time with more complex sites.
6. if the site becomes more complex later on, you move on to building more complex sites, or you’d like to transform your data, follow these steps:
  - Check out the Plugin Library to see if the source plugins and/or transformer plugins you’d like to use already exist
  - If they don’t exist, read the Plugin Authoring guide and consider building your own!
7. Gatsby uses GraphQL to enable components to declare the data they need. 
8. GraphQL was invented at Facebook
9. GraphQL is a query language (the QL part of its name).

## GraphQL

1. What if you want to change the site title in the future? You’d have to search for the title across all your components and edit each instance. This is both cumbersome and error-prone, especially for larger, more complex sites. Instead, you can store the title in one location and reference that location from other files; change the title in a single place, and Gatsby will pull your updated title into files that reference it.
2. The place for these common bits of data is the siteMetadata object in the gatsby-config.js file. Add your site title to the gatsby-config.js file
3. Now the site title is available to be queried; Add it to the about.js file using a page query:
4. Page queries live outside of the component definition — by convention at the end of a page component file — and are only available on page components.

### Static query
1. StaticQuery is a new API introduced in Gatsby v2 that allows non-page components (like your layout.js component), to retrieve data via GraphQL queries. Let’s use its newly introduced hook version — useStaticQuery.
2. For now, keep in mind that only pages can make page queries.
3. Non-page components, such as Layout, can use StaticQuery.

### how to pull data into your Gatsby site using GraphQL and source plugins

1. Before you learn about these plugins, however, you’ll want to know how to use something called GraphiQL, a tool that helps you structure your queries correctly.
2. GraphiQL is the GraphQL integrated development environment (IDE).
3. You can access it when your site’s development server is running—normally at http://localhost:8000/___graphql
4. Poke around the built-in Site “type” and see what fields are available on it — including the siteMetadata object you queried earlier. Try opening GraphiQL and play with your data! Press Ctrl + Space (or use Shift + Space as an alternate keyboard shortcut) to bring up the autocomplete window and Ctrl + Enter to run the GraphQL query.
5. The GraphiQL Explorer enables you to interactively construct full queries by clicking through available fields and inputs without the repetitive process of typing these queries out by hand.

### Source plugins
1. Source plugins fetch data from their source. E.g. the filesystem source plugin knows how to fetch data from the file system. The WordPress plugin knows how to fetch data from the WordPress API.
2. First, install the plugin at the root of the project: npm install --save gatsby-source-filesystem
3. Click the allFile dropdown. Position your cursor after allFile in the query area, and then type Ctrl + Enter. This will pre-fill a query for the id of each file. Press “Play” to run the query:

### Transformer plugins
1. In this tutorial, you’ll learn how transformer plugins transform the raw content brought by source plugins. The combination of source plugins and transformer plugins can handle all data sourcing and data transformation you might need when building a Gatsby site.
2. Gatsby supports transformer plugins which take raw content from source plugins and transform it into something more usable.
3. Add a markdown file to your site at src/pages/sweet-pandas-eating-sweets.md
4. learn how to transform it to HTML using transformer plugins and GraphQL.
5. gatsby-source-filesystem is always scanning for new files to be added and when they are, re-runs your queries.
6. Add a transformer plugin that can transform markdown files: npm install --save gatsby-transformer-remark
7. Select allMarkdownRemark again and run it as you did for allFile. You’ll see there the markdown file you recently added. Explore the fields that are available on the MarkdownRemark node.
8. Source plugins bring data into Gatsby’s data system and transformer plugins transform raw content brought by source plugins.
9. create a list of your markdown files on the front page.
10. you want to end up with a list of links on the front page pointing to each blog post.
11. But your one blog post looks a bit lonely. So let’s add another one at src/pages/pandas-and-bananas.md
12. Which looks great! Except… the order of the posts is wrong.
13. You can sort and filter nodes, set how many nodes to skip, and choose the limit of how many nodes to retrieve. With this powerful set of operators, you can select any data you want—in the format you need.
14. But you don’t want to just see excerpts, you want actual pages for your markdown files.
15. Gatsby is not limited to making pages from files like many static site generators. Gatsby lets you use GraphQL to query your data and map the query results to pages—all at build time. 

## Programmatically create pages from data

### creating slugs for pages

1. A ‘slug’ is the unique identifying part of a web address, such as the /tutorial/part-seven part of the page https://www.gatsbyjs.org/tutorial/part-seven/.
2. Creating new pages has two steps:
  - Generate the “path” or “slug” for the page.
  - Create the page.
3. Note: Often data sources will directly provide a slug or pathname for content — when working with one of those systems (e.g. a CMS), you don’t need to create the slugs yourself as you do with markdown files.
4. To create your markdown pages, you’ll learn to use two Gatsby APIs: onCreateNode and createPages.
5. To implement an API, you export a function with the name of the API from gatsby-node.js
6. This onCreateNode function will be called by Gatsby whenever a new node is created (or updated).
7. you will use this API to add slugs for your Markdown pages to MarkdownRemark nodes.
8. Change your function so it now only logs MarkdownRemark nodes.
9. You want to use each markdown file name to create the page slug. So pandas-and-bananas.md will become /pandas-and-bananas/
10.  To get it, you need to traverse the “node graph” to its parent File node, as File nodes contain data you need about files on disk.
11.  To do that, you’ll use the getNode() helper. Add it to onCreateNode’s function parameters, and call it to get the file node:
12.  the gatsby-source-filesystem plugin ships with a function for creating slugs. 
13. The function handles finding the parent File node along with creating the slug.
14. Now you can add your new slugs directly onto the MarkdownRemark nodes
15. createNodeField. This function allows you to create additional fields on nodes created by other plugins.
16. Only the original creator of a node can directly modify the node—all other plugins (including your gatsby-node.js) must use this function to create additional fields.

### Creating pages
1. the steps to programmatically creating pages are:
  - Query data with GraphQL
  - Map the query results to pages
2. You need one additional thing beyond a slug to create pages: a page template component.


## //////////// AUTO GENERATED ///////////////
<!-- AUTO-GENERATED-CONTENT:START (STARTER) -->
<p align="center">
  <a href="https://www.gatsbyjs.org">
    <img alt="Gatsby" src="https://www.gatsbyjs.org/monogram.svg" width="60" />
  </a>
</p>
<h1 align="center">
  Gatsby's hello-world starter
</h1>

Kick off your project with this hello-world boilerplate. This starter ships with the main Gatsby configuration files you might need to get up and running blazing fast with the blazing fast app generator for React.

_Have another more specific idea? You may want to check out our vibrant collection of [official and community-created starters](https://www.gatsbyjs.org/docs/gatsby-starters/)._

## 🚀 Quick start

1.  **Create a Gatsby site.**

    Use the Gatsby CLI to create a new site, specifying the hello-world starter.

    ```shell
    # create a new Gatsby site using the hello-world starter
    gatsby new my-hello-world-starter https://github.com/gatsbyjs/gatsby-starter-hello-world
    ```

1.  **Start developing.**

    Navigate into your new site’s directory and start it up.

    ```shell
    cd my-hello-world-starter/
    gatsby develop
    ```

1.  **Open the source code and start editing!**

    Your site is now running at `http://localhost:8000`!

    _Note: You'll also see a second link: _`http://localhost:8000/___graphql`_. This is a tool you can use to experiment with querying your data. Learn more about using this tool in the [Gatsby tutorial](https://www.gatsbyjs.org/tutorial/part-five/#introducing-graphiql)._

    Open the `my-hello-world-starter` directory in your code editor of choice and edit `src/pages/index.js`. Save your changes and the browser will update in real time!

## 🧐 What's inside?

A quick look at the top-level files and directories you'll see in a Gatsby project.

    .
    ├── node_modules
    ├── src
    ├── .gitignore
    ├── .prettierrc
    ├── gatsby-browser.js
    ├── gatsby-config.js
    ├── gatsby-node.js
    ├── gatsby-ssr.js
    ├── LICENSE
    ├── package-lock.json
    ├── package.json
    └── README.md

1.  **`/node_modules`**: This directory contains all of the modules of code that your project depends on (npm packages) are automatically installed.

2.  **`/src`**: This directory will contain all of the code related to what you will see on the front-end of your site (what you see in the browser) such as your site header or a page template. `src` is a convention for “source code”.

3.  **`.gitignore`**: This file tells git which files it should not track / not maintain a version history for.

4.  **`.prettierrc`**: This is a configuration file for [Prettier](https://prettier.io/). Prettier is a tool to help keep the formatting of your code consistent.

5.  **`gatsby-browser.js`**: This file is where Gatsby expects to find any usage of the [Gatsby browser APIs](https://www.gatsbyjs.org/docs/browser-apis/) (if any). These allow customization/extension of default Gatsby settings affecting the browser.

6.  **`gatsby-config.js`**: This is the main configuration file for a Gatsby site. This is where you can specify information about your site (metadata) like the site title and description, which Gatsby plugins you’d like to include, etc. (Check out the [config docs](https://www.gatsbyjs.org/docs/gatsby-config/) for more detail).

7.  **`gatsby-node.js`**: This file is where Gatsby expects to find any usage of the [Gatsby Node APIs](https://www.gatsbyjs.org/docs/node-apis/) (if any). These allow customization/extension of default Gatsby settings affecting pieces of the site build process.

8.  **`gatsby-ssr.js`**: This file is where Gatsby expects to find any usage of the [Gatsby server-side rendering APIs](https://www.gatsbyjs.org/docs/ssr-apis/) (if any). These allow customization of default Gatsby settings affecting server-side rendering.

9.  **`LICENSE`**: Gatsby is licensed under the MIT license.

10. **`package-lock.json`** (See `package.json` below, first). This is an automatically generated file based on the exact versions of your npm dependencies that were installed for your project. **(You won’t change this file directly).**

11. **`package.json`**: A manifest file for Node.js projects, which includes things like metadata (the project’s name, author, etc). This manifest is how npm knows which packages to install for your project.

12. **`README.md`**: A text file containing useful reference information about your project.

## 🎓 Learning Gatsby

Looking for more guidance? Full documentation for Gatsby lives [on the website](https://www.gatsbyjs.org/). Here are some places to start:

- **For most developers, we recommend starting with our [in-depth tutorial for creating a site with Gatsby](https://www.gatsbyjs.org/tutorial/).** It starts with zero assumptions about your level of ability and walks through every step of the process.

- **To dive straight into code samples, head [to our documentation](https://www.gatsbyjs.org/docs/).** In particular, check out the _Guides_, _API Reference_, and _Advanced Tutorials_ sections in the sidebar.

## 💫 Deploy

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/gatsbyjs/gatsby-starter-hello-world)

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/import/project?template=https://github.com/gatsbyjs/gatsby-starter-hello-world)

<!-- AUTO-GENERATED-CONTENT:END -->
