# Icelander : SEI Project Three

## Contents

- Project Overview
- Project Brief
- Technologies Used
- Project Timeline - 9 days
- Bugs
- Wins and Challenges
- Deployment
- Future Improvements
- Key Learnings

## Project Overview
This was a group project, with a brief to create a MERN stack app with a topic of our own choosing.
The concept for our app was driven by our common love for travelling. Iceland came up as one of the must see places to visit, so we thought this would be a great country to base our app around. The aim of the site is for users to explore self-driving tours available in Iceland. The user would be able to read up on daily itineraries for each package and explore on a map all of the locations they can visit (hotels, restaurants, attractions).

Once registered and logged in the user can rate a place on the map, and also save locations to their profile page.

Explore the full site **[here](https://icelander.netlify.app)**. A login is required to explore all features, feel free to use the below:

- email: karen@email
- password: pass

![Screenshot 2021-05-02 at 14 42 16](https://user-images.githubusercontent.com/77445688/116815390-413cb700-ab55-11eb-8c20-7cd8299fad6b.png)
![Screenshot 2021-05-05 at 12 59 06](https://user-images.githubusercontent.com/77445688/117137639-e2b84880-ada1-11eb-828e-3bfe0b0a1c3c.png)
![Screenshot 2021-05-05 at 12 59 24](https://user-images.githubusercontent.com/77445688/117137647-e77cfc80-ada1-11eb-8035-2424128db44e.png)

## Project Brief

- **Build a full-stack application** by making your own backend and your own front-end
- **Use an Express API** to serve your data from a Mongo database
- **Consume your API with a separate front-end** built with React
- **Be a complete product** build multiple relationships.

## Technologies Used

Back-end:

- Node.js
- Express
- Bcrypt
- Body-parser
- Mongoose/MongoDB
- jsonwebtoken

Frontend:

- React
- Axios
- Bootstrap
- SCSS
- React Mapbox GL

Development tools:

- Insomnia
- GitHub
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

The next two days were spent creating the backend together as a group. We wanted to ensure that everything was up and running, and any bugs we fixed together promptly so that each person could take a front end component to render over the weeked. This was also our first time using the development branch and feature branch, and therefore wanted to avoid as many merge conflicts as possible. We decided a leader between us, and then the rest of us followed the steps of cloning the original repo.

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
        if (!onePlace) throw new Error()
        return res.status(200).json(onePlace)
      } catch (err) {
        console.log(err)
        return res.status(404).json({ message: 'Not Found' })
      }

We also worked on the authorization controller for the user registration and login requests. We used jwt to create the token which would then be displayed in Insomnia and used to access certain features of the site.

After we tested that all routes worked in Insomnia, we installed the front end section together and attempted rendering an interactive map which would be later used to render all places as icons on the map. We installed the front end by running ```npx create-react-app client —template cra-template-ga-ldn-projects```, and then updated the port to match the backend.

For the interactive map, we used the MapBox API and saved our token in our .env file. The Viewport was set to the long and lat of Icleand, and each place that was seeded was renderd as an icon on the map. If you clicked on the icon, a card appears with the three tabs with information about that place. This was the first time using Bootstrap, so it took some time getting used the imports at the top from Bootstrap in order for the Card to render.

![Screenshot 2021-05-04 at 09 32 35](https://user-images.githubusercontent.com/77445688/116978524-b9bd8800-acbb-11eb-83eb-4fe8ffb5fbba.png)


### Day Four and Five - Frontend

As we managed to render the main map together before the weekend, we then all took ownership of a seperate front end component to work on over the weekend, as well collecting more data to seed for the final version.

I was responsible for creating the package show page. This page would be used to display the daily itinerary of the tour which would be rendered on top of the map, with the icons of the places that relate to that package only. The aim was to use Bootstrap Carousel to render each day seperately.

Two seperate components were created for this page. The main page which had the map and the GET request to the API to render the iconc of the places relating to the package. The second component was used to pass through props and format the intinerary in the carousel, which was the ShowPackageTile component.

      const [packages, setPackage] = useState(null)

       useEffect(() => {
         const getData = async () => {
           const { data } = await axios.get('/api/packages')
           setPackage(data)
         }
         getData()
       }, []

       return (
         <>
           <div className="map-container">
           
           <ReactMapGL
               {...viewPort}
               onViewportChange={(viewPort) => setViewPort(viewPort)}
               mapboxApiAccessToken={process.env.REACT_APP_MAPBOX_ACCESS_TOKEN}
               height='100%'
               width='100%'
               mapStyle='mapbox://styles/mapbox/streets-v11' >
             </ReactMapGL>
             
             <div className="map-controller"> 
               {packages.map(item => (
                 <ShowPackageTile
                   key={item._id}
                   { ...packages}/>
               )) }
             </div> 
           </div> 
         </>
      )
    
This was the first major hurdle I had come across is that the above and below code meant that this component was rendering all the items in the array and I was not able to split out places using the array index by passing through props. The aim was to be able to render each day seperately.

        import React from 'react'
        import Media from 'react-bootstrap/Media'


        const ShowPackageTile = ( props ) => {

           return (

                <Media>
                 <Media.Body>
                   <h4>{props.title}</h4>
                   <h5>{props.subTitle}</h5>
                   <h6>{props.day[0]}</h6>
                   <p>   
                     <img
                       width={250}
                       height={150}
                       className="float-left mr-2 mb-1"
                       src={props.image[0]}
                       alt="Generic placeholder"
                     />
                     {props.description[0]}
                   </p>
                 </Media.Body>
               </Media>
            )

After a while of not being able to fix this solution, I spent the rest of my time working on the data that we would need to seed at the end as well as styling the map and the container that would hold the info of the intinerary.

### Day Six and Seven - Troubleshooting

This day was spent trying find a way to connect the places to the packages model so that we could render them both on the front end. Two of us worked together on the ShowPackage page whilst the other members worked on rendering a list of packages in a separate component. We had to add more fields to each of the models for the relationship to work. The places model was refactored to the below:

**Places Model Update:**

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
      star: { type: String },
      day1: { type: Boolean, required: true },
      day2: { type: Boolean, required: true },
      day3: { type: Boolean, required: true },
      day4: { type: Boolean, required: true },
      day5: { type: Boolean, required: true },
      day6: { type: Boolean, required: true },
      day7: { type: Boolean, required: true },
      day8: { type: Boolean, required: true },
      day9: { type: Boolean, required: true },
      day10: { type: Boolean, required: true },
      packageNumber: { type: Number, required: true },
      packages: [{ type: Number, required: true }],
      packageName: [{ type: String, required: true }],
      ratings: [ratingSchema]
    })
    
This then meant that we were able to filter the data on the front end to only render the places with the days included in that package. The only negative to this was that if this place was to be used for a different package on a different day then that place would need to be duplicated with the correct true/false booleans for the day and package name.

The code on the front end component was also updated to filter through the array of places not packages, and onnly return the places that match the package id which was passed into the component using Params.

    const ShowPage = () => {

      const { id } = useParams()

      const [locations, setLocations] = useState([])

      useEffect(() => {
        const getData = async () => {
          const { data } = await axios.get('/api/places')
          const packageData = data.filter(item => {
            return item.packages.includes(parseInt(id))
          })
          setLocations(packageData)
        }
        getData()
      }, [])
