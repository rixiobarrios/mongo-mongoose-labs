Title: Mongoose Vampires<br>
Type: Lab<br>
Duration: 1 + hours <br>
Creator: WDI-Meeseeks <br>
Adapted by: Hou Chia, Kristyn Bryan, Karolin Rafalski<br>

---

# Mongoose Vampires

For this lab, you will be using some of the mongoose commands that you learned today and you will be **reading the documents** to find **new** queries to complete the following activities. Researching queries and implementing them is a big part of this lab!

![mongoose](https://s-media-cache-ak0.pinimg.com/564x/ee/b7/a9/eeb7a99383582d53e65ffcc0e4a225bd.jpg)

# Resources

Utilize the following resources to research the commands you will need:

- Your notes from today
- [Mongoose Documentation](https://mongoosejs.com/docs/guide.html)
- Google to find Stack Overflow articles and more

# Setup

1. Start your mongo server with `mongod` in terminal
1. navigate to this directory in terminal and `npm i --save mongoose` to install `mongoose`
1. open the project in vscode, you'll be working with some starter code in the `models` folder and the `app.js` file

# What is a schema?

A schema is a way to organize, ahead of time, what a group of data is going to look like. This can be at various levels of a database depending on what kind of databases you are using.

Mongo, is schema-less on the database level. It doesn't care what the data looks like and will take in virtually anything as long as it's syntactically correct.

## Why they are important?

Even when you are using MongoDB, an inherently schema-less database, a schema can be very helpful. It helps control what is going into the database so that you can both know what is going into it, and to make validations. Note that with MongoDB, even if a piece of data is not a part of your original schema, you can still store it.

## Mongoose

This is where mongoose comes in. Instead of manually making sure everything we are putting into our database makes sense and conforms to some type of structure, Mongoose allows us to define schemas.

Mongoose, in the background, can enforce these schemas (as strictly as you like) in order to make sense of the data going into the database and to allow validation. It provides powerful and simple to use tools to do this.

# The Exercise

## Building a Schema

Lets design a schema using mongoose and then use it to create some documents and eventually query for those documents.

Our vampire collection will look something like this:

```javascript
var vampire = {
  name: 'Chocula',
  title: 'Count'
  hair_color: 'brown',
  eye_color: 'brown',
  dob: new Date(1971, 2, 13, 7, 47),
  loves: ['cereal','marshmallows'],
  location: 'Minneapolis, Minnesota, US',
  gender: 'm',
  victims: 2,
}
```

1. Build a vampire **schema** and **model** that matches the object above inside the `models/vampires.js` file

1. Go to the Mongoose documentation to learn more about validations and defaults: http://mongoosejs.com/docs/api.html

1. The **name field is required**, so make sure that the schema accommodates for that.

1. Also, **no vampire will have less than 0 victims**, so add that into the schema as a validation.

1. Lastly, set the **default value of the hair color to blonde**.

## Inserting Seed Data Using Mongoose

Insert into the database using **create** method:

### Add the vampire data that we gave you

1. We have required `seed_vampires.js` for you in the `app.js` file. This is an array of Vampires for you to insert into your database.

1. Write this command and run it **ONCE** in `app.js` - once you are done, comment it out or else every time you run it it will make duplicates of your data.

```javascript
Vampire.insertMany(seedData, (err, vampires) => {
  if (err) {
    console.log(err);
  }
  console.log("added provided vampire data", vampires);
  mongoose.connection.close();
});
```

1. remember run your file with `node app.js`

### Add some new vampire data

1. Using the create method, create 4 new vampires with any qualities that you like two should be male and two should be female.

## Querying

### Select by comparison

Write a different query for each of the following:

1. Find all the vampires that that are females
2. have greater than 500 victims
3. have fewer than or equal to 150 victims
4. have a victim count is not equal to 210234
5. have greater than 150 AND fewer than 500 victims

### Select by exists or does not exist

Select all the vampires that:

1. have a key of 'title'
2. do not have a key of 'victims'
3. have a title AND no victims
4. have victims AND the victims they have are greater than 1000

### Select with OR

Select all the vampires that:

1. are from New York, New York, US or New Orleans, Louisiana, US
2. love brooding or being tragic
3. have more than 1000 victims or love marshmallows
4. have red hair or green eyes

### Before you continue on to part two, you should know that Mongoose has some sweet helper functions that can make all this a little easier. See below.

Mongoose's default find gives you an array of objects.  But what if you know you only want one object?  These convenience methods just give you one object without the usual array surrounding it.

```javascript
Article.findById('5757191bce5579b805705900', (err, article)=>{
  console.log(article);
});
```
```javascript
Article.findOne({ author : 'Matt' }, (err, article)=>{
  console.log(article);
});
```
```javascript
Article.findByIdAndUpdate(
  '5757191bce5579b805705900', // id of what to update
  { $set: { author: 'Matthew' } }, // how to update it
  { new : true }, // tells findOneAndUpdate to return modified article, not the original
  (err, article)=>{
    console.log(article);
  });
});
```
```javascript
Article.findOneAndUpdate(
  { author: 'Matt' }, // search criteria of what to update
  { $set: { author: 'Matthew' } }, // how to update it
  { new : true }, // tells findOneAndUpdate to return modified article, not the original
  (err, article)=>{
    console.log(article);
  });
});
```
```javascript
Article.findByIdAndRemove('5757191bce5579b805705900', (err, article)=>{
  console.log(article); // log article that was removed
});
```
```javascript

Article.findOneAndRemove({ author : 'Matt' }, (err, article)=>{
  console.log(article); // log article that was removed
});
```

### Select objects that match one of several values

Select all the vampires that:

1. love either frilly shirtsleeves or frilly collars
2. love brooding
3. love at least one of the following: appearing innocent, trickery, lurking in rotting mansions, R&B music

### Negative Selection

Select all vampires that:

1. love ribbons but do not have brown eyes
2. are not from Rome
3. do not love any of the following: [fancy cloaks, frilly shirtsleeves, appearing innocent, being tragic, brooding]
4. have not killed more than 200 people

## Replace

1. replace the vampire called 'Claudia' with a vampire called 'Eve'. 'Eve' will have a key called 'portrayed_by' with the value 'Tilda Swinton'
2. replace the first male vampire with another whose name is 'Guy Man', and who has a key 'is_actually' with the value 'were-lizard'

## Update

1. Update 'Eve' to have a gender of 'm'
2. Rename 'Eve's' name field to 'moniker'
3. We now no longer want to categorize female gender as "f", but rather as **fems**. Update all females so that the they are of gender "fems".

## Remove

1. Remove a single document wherein the hair_color is 'brown'
2. We found out that the vampires with the blue eyes were just fakes! Let's remove all the vampires who have blue eyes from our database.
   <hr>
