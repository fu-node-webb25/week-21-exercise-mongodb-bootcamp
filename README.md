# Vecka 21: MongoDB Bootcamp

Nedanstående övningar förutsätter att ni skapat ett konto på MongoDB, alt. loggat in med ert Google-konto.

## Övning 1: Skapa en collection
1. Inne på MongoDB Atlas (webben), bege dig till dina **Collections** och klicka på knappen **+ Create Database**.
2. Ge din databas namnet **bookstore** och din collection namnet **books** innan du klickar på **create**.
3. Klicka på **Insert Document**, och välj måsvingarna under **View**. Klistra in nedanstående objekt och klicka på **Insert**

```
{
  "title" : "The spy who came in from the cold",
  "author" : "John le Carré"
}
```


## Övning 2: Custom Role
1. Logga in på MongoDBAtlas och klicka på **Database Access** i menyn till vänster.
2. Klicka på fliken **Custom Roles**.
3. Klicka på den gröna knappen där det står **Add new custom role**.
4. Ge rollen namnet **booksReadAndWrite**.
5. Under **Actions and Roles** klickar du endast i **Collection Roles**, därefter fyller du i namnet på den databas och collection som du skapade i övning 1.

## Övning 3: Database Users
1. Stanna kvar inne på **Database Access**, men klicka nu in dig på fliken **Database Users**.
2. Klicka på den gröna knappen där det står **Add new database user**.
3. I modalen som dyker upp säkerställer du att **Password** är vald **Authentication Method**.
4. Därefter skall du ange username och password för din databasanvändare, för övningens skull låter du **booksUser** både som username och password.
5. En bit längre ner öppnar du upp fliken **Custom Roles**, klickar på **Add Custom Role**, och i dropdownlistan som dyker upp väljer du din nyligen skapade **booksReadAndWrite**-roll.

## Övning 4: MongoDBCompass
1. Klicka på **Clusters** i vänstermenyn.
2. Bredvid namnet på ditt Cluster finns knappen **Connect**. Klicka på den.
3. I modalen som nu öppnades klickar du på **Compass**.
4. Kopiera den **Connection String** som du kan se i del 2 av modalen.
5. Öppna MongoBCompass och klicka på **Add New Connection**.
6. Under **URI** klistrar du in din kopierade **Connection String**. Se till att ersätta ``<db_username>`` och ``db_password`` med de uppgifter som du gav databasanvändaren, alltså **booksUser**, innan du klickar på **Connect**.
7. Klicka dig fram till din collection, därefter klickar du på **Add Data** följt utav **Insert Document**. Klistra in nedanstående objekt och klicka på **Insert**.

```
{
  "_id": {"$oid": "68273baaa32353a34549161c"},
  "title" : "Ulysseus",
  "author" : "James Joyce"
}
```

## Övning 5: MongoDB <3 Express

Här nämner jag ingen om någon ``.env``-fil, men vill ni använda det istället för att klistra in din **Connection String** rakt in i koden så kör på!

1. Skapa upp en vanlig expresserver och kör igång den.
2. Installera **mongoose** genom att köra ``npm i mongoose`` i terminalen, och importera det sedan in i din kod.
3. Följ setg 1 och 2 i Övning 4, och klicka därefter på **Drivers** i modalen. Kopiera din **Connection String**.
4. Skapa en connection till MongoDB genom att lägga in följande i din config-del av servern. Glöm inte att skriva in rätt username och password i din **Connection String**, samt att lägga till namnet på din databas precis innan ```?retryWrites```i strängen.
 ```
 mongoose.connect(
  'mongodb+srv://booksUser:booksUser@cluster0.xxxxx.mongodb.net/bookstore?retryWrites=true&w=majority' // Stoppa in din egen connection string här
);
 const database = mongoose.connection;
 ```
5. Felhantera genom att lägga in en emitlyssnare som lyssnar efter fel i databasuppkopplingen. Se nedan:
```
database.on('error', (error) => console.log(error));
```
5. Lägg ävn en emitlyssnare som endast körs EN gång och som lyssnar efter en uppkoppling. När uppkopplingen finns loggar du ut ett meddelande, samt startar din server. Se nedan:
```
database.once('connected', () => {
    console.log('DB Connected');
    app.listen(PORT, () => {
        console.log(`Server is running on port ${PORT}`);
    });
});
```
6. Starta din igång din utvecklingsserver och kontrollera i terminalen så att allt startar.

## Övning 6: Koda mot din databas
1. Börja med att skapa ett schema och en modell som du döper till **Book**.
```
const mongoose = require('mongoose');

const bookSchema = new mongoose.Schema({
    title: String,
    author: String
});

const Book = mongoose.model('Book', bookSchema);
```
2. Skapa ett **GET-anrop** som hämtar alla böcker i databasen. Tips! Använd metoden .find() på ditt Book-objekt.
3. Skapa ett **GET-anrop** som hämtar ut en bok baserat på dess ID från databasen. Tips! Använd metoden .findById(*id*) på ditt Book-objekt.
4. Skapa ett **POST-anrop** som tar emot en bok i **req.body** och som lägger till boken i databasen. Tips! Använd metoden .create(*req.body*) på ditt Book-objekt.
5. Fortsätt leka runt lite med din databas och testa att använda metoderna .findByIdAndUpdate(), samt .findByIdAndDelete().

## Bonus!
Läs på om metoderna **.findOne()**, **.findOneAndUpdate()** och **.findOneAndDelete()**. På vilket sätt skiljer de sig från motsvarande metoder ovan? Även metoderna **.insertMany()** och **.exists()** kan vara nyttig att känna till.
