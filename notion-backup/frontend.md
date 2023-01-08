# frontend

well this seems like a thing to do, dont like hopping up to youtube for easy stuff every minute or two.

### 1.`working with Mantine`

AUTH

![Screen Shot 2022-12-14 at 5.05.55 PM.png](frontend%20915beb8b0e664d3c9adbe8f66bb2e95c/Screen_Shot_2022-12-14_at_5.05.55_PM.png)

 

`the retarded way, but works.`

example elements:

![Untitled](frontend%20915beb8b0e664d3c9adbe8f66bb2e95c/Untitled.png)

   `header and stats`

make a component, solve its dependency issues, and import import in `index.tsx`

then add mock data(first copy from `mantine.dev`) like-

```tsx
<HeaderMiddle links={[
        {
          "link": "/about",
          "label": "Home"
        },
        {
          "link": "/learn",
          "label": "Features"
        },
        {
          "link": "/pricing",
          "label": "Pricing"
        }

      ]}></HeaderMiddle>
      <h3 style={{
        
    paddingLeft: '20px',
    paddingTop: '20px',
    boxSizing: 'content-box',
  }} >Track your impermanant losses</h3>
      <StatsGrid data={[
    {
      "title": "Revenue",
      "icon": "receipt",
      "value": "13,456",
      "diff": 34
    },
    {
      "title": "Profit",
      "icon": "coin",
      "value": "4,145",
      "diff": -13
    },
    {
      "title": "Coupons usage",
      "icon": "discount",
      "value": "745",
      "diff": 18
    },
    {
      "title": "New customers",
      "icon": "user",
      "value": "188",
      "diff": -30
    }
  ]} ></StatsGrid>
```

### 2.`working with apis`

well, its easy imagine you gotta fetch data from a github api and represent it as containers

something like this-

![Untitled](frontend%20915beb8b0e664d3c9adbe8f66bb2e95c/Untitled%201.png)

First get the url and see how the data is jsonified there,

url: 

[](https://api.github.com/users?limit=3)

First write a async await fetch function that gets data, stringifies it, and sets data to a func setUsers().

```tsx
const getUsers = async () => {
    const response = await fetch("https://api.github.com/users");
    const data = await response.json();
    setUsers(data);
  };
```

using useState() to setUsers everytime we get new data, and actually to map the data.

```tsx
const [users, setUsers] = useState([]);
```

use the useEffect hook, to call this function whenever the page’s reloaded so we can get the data once every reload.

```tsx
useEffect(() => {c
    getUsers();
  }, []);
```

Now for the interesting part, we have the data but we gotta map it as well, so we can represent as we want and not as json.

in the return tag of your component,

```tsx
{
users.map((user) => {
       //this 

        return (
          <>
            use the card comp of mantine here, 
						make sure you get the dependencies right.

          </>
        );
      })
}
```

how to actually subsitute data-

check the api endpoint-

```latex
{
    "login": "mojombo",
    "id": 1,
    "avatar_url": "https://avatars.githubusercontent.com/u/1?v=4",
    "type": "User",
    "site_admin": false
  },
  {
    "login": "defunkt",
    "id": 2,
    "node_id": "MDQ6VXNlcjI=",
    "avatar_url": "https://avatars.githubusercontent.com/u/2?v=4",
    "type": "User",
    "site_admin": false
  },
```

the data is coming in the above format, earlier we have used a mapping which iterates through this data and here every data is defined by a parameter `user` defined earlier in the mapping,

so we use `user.id` to get the id, `user.avatar_url` to get the image.

```tsx
<div>
    <Image src={//@ts-ignore 
       user.avatar_url} />
		<h1>{user.id}</h1>
</div>
```

this is how you fetch and map data as you like it,

### 3. `working with axios`

a better way to work with apis 

`yarn add axios`

now you get using promises, `.get` and `.then` define the css as well and literally itne code mein you get a blog website.

```tsx
import { useState, useEffect } from "react";
import "./App.css";
import axios from "axios.jsx";

const App = () => {
  const [myData, setMyData] = useState([]);
  const [isError, setIsError] = useState("");

  // using Promises.
  useEffect(() => {
    axios
      .get("https://jsonplaceholder.typicode.com/posts")
      .then((response) => setMyData(response.data))
      .catch((error) => setIsError(error.message));
  }, []);

  return (
    <>
      <h1>Axios Tutorial</h1>
      {isError !== "" && <h2>{isError}</h2>}

      <div className="grid">
        {myData.slice(0, 9).map((post) => {
          const { body, id, title } = post;
          return (
            <div key={id} className="card">
              <h2>{title.slice(0, 15).toUpperCase()}</h2>
              <p>{body.slice(0, 100)}</p>
            </div>
          );
        })}
      </div>
    </>
  );
};

export default App;
```

defining `css` in App.css

![Untitled](frontend%20915beb8b0e664d3c9adbe8f66bb2e95c/Untitled%202.png)

### 4.`Pagination and rows/containers`

### 5.**`Styling`**

learn tailwind, though here are some basics,

`span` this basically allows you to use styles(with a ton of css properties) and have multiple elements (like you can have multiple spans) 

`group` you can have multiple spans inside a grp like i used here in the header.

(this is done inside an appshell to have foter header all in one component basically)

```tsx
header={<Header height={60} p='md'>
        <Group position="apart" spacing='xl'>
            <Text size='xl'>
            <span style={
            {
                fontWeight: "bolder",
                fontSize: "1.2rem",
            }
        }>Alpaca</span>    Activity.
            </Text>
        
        <span style={{
            fontSize: "1.3rem",
        }}>🦙</span>
        </Group>
      </Header>}
```

looks like this:

![Untitled](frontend%20915beb8b0e664d3c9adbe8f66bb2e95c/Untitled%203.png)

`Using AppShell`

## 6. `dealing with props`

to pass data from index.ts from the component’s tag to the component itself for mapping, use destructured props

example:

`index.tsx`

```tsx
return(
<FeaturesCard name= "hellomf"></FeaturesCard>
);
```

`component file`

```tsx
export function FeaturesCard({...props}) {
```

`component’s return tag`

```tsx
<Text size="xs" color="dimmed">
            {props.name}
</Text>
```

launches ka map, uske andar we’ll pass <featurecard name ={launches.mission_name} image ={launches.image} />

but this isnt working

issue, launches is an array of 15 elements with different data,

fuck i was right let’s go.

## `Working with graph ql and mapping`

we wanna create something like this,

![Untitled](frontend%20915beb8b0e664d3c9adbe8f66bb2e95c/Untitled%204.png)

```tsx
yarn add @apollo/client graphql (get dependencies)
```

pick up mantine card element

```tsx
https://ui.mantine.dev/category/app-cards
```

define a schema from the graph explorer [https://api.spacex.land/graphql/](https://api.spacex.land/graphql/)

write `getStaticprop`() which will have the graphql query and return data as a prop.

```tsx
export async function getStaticProps() {
  //this func is first getting the data through the graphql client,
  // storing it in destructured {data}

  const Client = new ApolloClient({
    uri: `https://api.spacex.land/graphql/`,
    cache: new InMemoryCache(),
  });

  const { data } = await Client.query({
    query: gql`
      query getLaunches {
        launchesPast(limit: 4, offset: 30) {
          mission_name
          launch_date_local
          launch_date_utc
          launch_site {
            site_name_long
          }
          links {
            article_link
            video_link
            mission_patch_small
            mission_patch
          }
          rocket {
            rocket_name
            first_stage {
              cores {
                flight
              }
            }
          }
          id
        }
      }
    `,
  });

  console.log("data", data);
  //what does props do here?
  return {
    props: {
      launches: data.launchesPast,
    },
  };
}
```

now in main function, pass launches as a destructured prop

```tsx
export default function Home({launches}) {}
```

map the data from launches prop to new props which will go inside the `card.tsx` file

```tsx
{launches.map(
          (launch) => {
            return (
              <FeaturesCard
                name={launch.mission_name}
                image={launch.links.mission_patch_small}
                date={launch.launch_date_utc}
                link={launch.links.video_link}
                rocket={launch.rocket.rocket_name}
              ></FeaturesCard>
            );
          }
        )}
//features card is the component in card.tsx
//yahan as props paas kiya ja rha data 
//jo actually mein khud props ki tarah card.tsx mein pass hoga
```

in `card.tsx` take props in

```tsx
export function FeaturesCard({...props}) {}
```

now map it as `props.name, props.image` blah blah

```tsx
return (
    <Card withBorder radius="md" className={classes.card}>
      <Card.Section className={classes.imageSection}>
        <Image src={props.image} alt="no image"/>
      </Card.Section>

      <Group position="apart" mt="md">
        <div>
          <Text weight={500}>{props.name}</Text>
          <Text size="md" color="dimmed">
            {props.date}
          </Text>
        </div>
        
      </Group>
      <br></br>

      <Card.Section className={classes.section}>
        <Group spacing={30} position= "apart">
          <div>
            <Text size="xl" weight={700} sx={{ lineHeight: 1 }}>
             {props.rocket}
            </Text>
            <Text
              size="sm"
              color="dimmed"
              weight={500}
              sx={{ lineHeight: 1 }}
              mt={3}
            >
              Rocket
            </Text>
          </div>

          <Button radius="xl" >
            <a href={props.link}>Watch</a>
          </Button>
        </Group>
      </Card.Section>
    </Card>
  );
```

okay get acc details through metamask, or just abhi get acc through a text field, 

again, i cannot actually map the data, i can console.log it i need to get support.

hey, im working on the [https://thegraph.com/hosted-service/subgraph/messari/sushiswap-polygon](https://thegraph.com/hosted-service/subgraph/messari/sushiswap-polygon) subgraph, 

im having problems mapping the fetched data to the ui components, this is my first time using the graph, so any insights would be very helpful, thanks

this is the query function

```tsx
export async function getStaticProps() {

  const Client = new ApolloClient({
    uri: `https://api.thegraph.com/subgraphs/name/messari/sushiswap-polygon`,
    cache: new InMemoryCache(),
  });

  const { data } = await Client.query({
    query: gql`
    {
      deposits(where: {
        from: "0xd5da26eae4448e5be3a1133bad3e7a76e86efeb3"
      }) {
        timestamp
        from
        inputTokens { id name }
        outputToken { id name }
        inputTokenAmounts
        outputTokenAmount
        amountUSD
      }
      
        withdraws(where: {
        from: "0xd5da26eae4448e5be3a1133bad3e7a76e86efeb3"
      }) {
        timestamp
        from
        inputTokens { id name }
        outputToken { id name }
        inputTokenAmounts
        outputTokenAmount
        amountUSD
      }
    }
    `,
  });

  console.log("data", data.deposits);
 
  return {
    props: {
      launches: data.deposits,
    },
  };
}
```

map code

```tsx
{
      launches.map(
        (launch) => {
          return (
            <h3>{launch.timestamp}</h3>
          );
        }
      )}
```

here’s how im taking the props

```tsx
export default function Home(...launches) {
```

i should be able to access the specific data like {launch.timestamp} right? or am i doing something wrong?

`importing image in next.js`

```tsx
import Image from 'next/image'
import React from 'react'
import gradbox1 from '../public/images/gradbox1.png'

const Titlehead = () => {
  return (
    <div>
          <Image src={gradbox1} alt={'ok'} fill/>
    </div>
  )
}

export default Titlehead
```
