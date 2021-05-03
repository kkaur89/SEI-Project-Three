# Icelander : SEI Project Three

## Contents

- Project Overview
- Project Brief
- Technologies Used
- Project Timeline - 9 days
- Bugs
- Wins and Challenges
- Future Improvements
- Key Learnings

## Project Overview
This was a group project, with a brief to create a MERN stack app with a topic of our own choosing.
The concept for our app was driven by our common love for travelling. Iceland came up as one of the must see places to visit, so we thought this would be a great country to base our app around. The aim of the site is for users to explore self-driving tours available in Iceland. The user would be able to read up on daily itineraries for each package and explore on a map all of the locations they can visit (hotels, restaurants, attractions).

Once registered and logged in the user can rate a place on the map, and also save locations to their profile page.

Explore the full site **[here](https://icelander.netlify.app)**. You will need to a log in to explore all features, feel free to use the below:

- email: karen@email
- password: pass

![Screenshot 2021-05-02 at 14 42 16](https://user-images.githubusercontent.com/77445688/116815390-413cb700-ab55-11eb-8c20-7cd8299fad6b.png)

## Project Brief

- **Build a full-stack application** by making your own backend and your own front-end
- **Use an Express API** to serve your data from a Mongo database
- **Consume your API with a separate front-end** built with React
- **Be a complete product** build multiple relationships.

## Technologies Used

Back-end:

- Node.js
- Mongodb
- Express
- Bcrypt
- Body-parser
- Mongoose/MoongoDB
- jsonwebtoken

Frontend:

- React
- Axios
- Bootstrap
- SCSS
- Http-proxy-middleware
- Nodemon
- React Router Dom
- React Mapbox GL
- React rating stars component

Development tools:

- VS code
- NPM
- Insomnia
- GitHub
- Google Chrome dev tools
- Heroku (deployment)
- Trello Board (planning and timeline)
- Excalidraw (wireframing)


## Project Timeline - 9 days

### Day One - Planning

The first day was spent on planning the project. We created a google doc, outlining a plan for the key areas of focus:

- MVP - What are the basic features we want the user to be able to access?
- Components that we would then need in the front end to support these features, and how would they link to each other.
- Building the API - all the endpoints we would need, and what the dataset would look like for each endpoint.
- The models in the backend and the relationships between them.
- Packages that we would need to support our app.

The next step was to create a wireframe based on the MVP features we landed on. We used Excalidraw for the first time, this app was perfect for allowing us all to be collaborative, we took a few components each and then reviewed the wireframe together and made any necessary updates.

![Iceland Wireframe  (1)](https://user-images.githubusercontent.com/77445688/116818420-1ce7d700-ab63-11eb-863f-65989e1800b5.png)


Once we were signed off, we then got together and created a Trello board for the project outline all the tasks needed to achieve MVP and also added additional features if we had time. After creating the Trello board, we then all took a dataset each needed to create a list of places which would then be used to create the tour packages. We split the data by Hotels, Restaurants, Volcanos and Attractions, ensuring we had the longitude and latitude of each so we could render the location on the map.


![Screenshot 2021-05-02 at 16 34 34](https://user-images.githubusercontent.com/77445688/116818636-566d1200-ab64-11eb-87ec-c46581400a06.png)


### Day Two and Three - Backend 

The next two days were spent creating the backend together as a group. We wanted to ensure that everything was up and running, and any bugs we fixed together promptly so that each person could take a front end component to render over the weeked.

The project started by installing all of the packages and dependencies that we would need for the app, ```yarn init, yarn add express, yarn add mongoose``` . To add our basic config, we created an index.js file which held all of our functionality for the server.

We then went on to create the controllers, models and routes for the app. We created the three key Models; Places, Packages and Users:


**Places Model**

    const placeSchema = new mongoose.Schema({
      nameOfDestination: { type: String, required: true },
      typeOfDestination: { type: String, required: true },
      longitude: { type: Number, required: true },
      latitude: { type: Number, required: true },
      description: { type: String, required: true },
      image: { type: String, required: true },
      icon: { type: String, required: true },
      packageId: { type: Number },
      winterAccess: { type: Boolean, required: true },
      star: { type: String }
    }
    
**Packages Model**

    const packageSchema = new mongoose.Schema({
      name: { type: String, required: true, unique: true },
      price: { type: Number, required: true },
      description: { type: String },
      duration: { type: Number, required: true },
      image: { type: String, required: true },
      reviewRating: { type: Number },
      season: { type: String },
      packageNumber: { type: Number, required: true, unique: true }
    })
    
**User Model**  - For the user model we also added the pre validate/save functions using bcrypt for the password, as well as the virtual fields needed for password confirmation.

    const userSchema = new mongoose.Schema({
      username: { type: String, required: true, unique: true, maxlength: 40 },
      email: { type: String, required: true, unique: true },
      password: { type: String, required: true },
    })


We then moved on to creating our controllers and routes so that we could seed some data and test the requests in Insomnia. Each model had its own controller starting with a GET request got each or all packages/places:

**Places Controller**

    import Place from '../models/place.js'

    //! INDEX Route
    export const getAllPlaces = async (_req, res) => {
      try {
        const placesLibrary = await Place.find()
        console.log('PLACES LIBRARY >>>', placesLibrary)
        return res.status(200).json(placesLibrary)
      } catch (err) {
        console.log(err)
        return res.status(404).json({ message: err.message })
      }
    }

    //! INDIVIDUAL Place Route
    export const getOnePlace = async (req, res) => {
      try {
        const { id } = req.params
        const onePlace = await Place.findById(id)
        console.log('THE PLACE WE WANT >>>', onePlace)
        if (!onePlace) throw new Error()
        return res.status(200).json(onePlace)
      } catch (err) {
        console.log(err)
        return res.status(404).json({ message: 'Not Found' })
      }

We also worked on the authorization controller for the user registration and login requests. We used jwt to create the token which would then be displayed in Insomnia and used to access certain features of the site.

After we tested that all routes worked in Insomnia, we installed the front end section together and attempted rendering an interactive map which would be later used to render all places as icons on the map. We installed the front end by running ```npx create-react-app client —template cra-template-ga-ldn-projects```, and then updated the port to match the backend.

For the interactive map, we used the MapBox API and saved our token in our .env file.





