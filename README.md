# Express + Sequelize with Relationships (The Sequel)

## Objectives

By the end of this, developers should be able to:

- Create Relationships between Sequelize Models
- Add API endpoints to reflect Model Relationships
- Code refactoring with Single Responsibility Principal a.k.a. SRP

## Let's Get To Work

Let's create our 2nd Model and update the new model and migration files to make sure they work properly with PostgreSQL.

```bash
node_modules/.bin/sequelize model:create --name Article --attributes "title:string, content:text"
```

Next step, run:

```bash
node_modules/.bin/sequelize db:migrate
```

We should now have our 2nd table created for us.

That's great, but now we have a new table that is empty. Meaning there is nothing to show.

So we will create another seed file to add a few Articles to the Article table.

```bash
node_modules/.bin/sequelize seed:create --name articles
```

Update People Seeder File:

**Up:**

```javascript
return queryInterface.bulkInsert('articles', [{
  title: "Hello World!",
  content: "This is a test article.",
  created_at: new Date,
  updated_at: new Date
}], {});
```

**Down:**

```javascript
return queryInterface.bulkDelete('articles', null, {});
```

With that done, let's seed the **db**:

```bash
node_modules/.bin/sequelize db:seed:all
```

Let's see what it added to our database.

```bash
psql -h localhost -U "postgres" express_api_database_development

\l # List DB
\d # List table
\d # "TABLE_NAME" List Table columns
```

That's great, now let's finish this by getting the data from the database and showing it to the user like we have already done before.

<details>
  <summary>Solution:</summary>

```javascript
app.get('/api/articles', (req, res) => {
  models.Article.findAll()
    .then(articles => {
      res.status(200).json({ articles: articles });
    })
    .catch(e => console.log(e));
});
```
</details>

Know that we have another real API endpoint that can read from the database and show a list of articles to the user. Let's do another version of it where the user can ask for information about a specific article.

How would we do that?

<details>
  <summary>Solution:</summary>

```javascript
app.get('/api/article/:id', (req, res) => {
  models.Article.findByPk(req.params.id).then(article => {
    res.status(200).json({ article: article });
  })
  .catch(e => console.log(e));
});
```
</details>

## Lab

**To Do:**

1. Update existing Article
1. Delete existing Article