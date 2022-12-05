## Project Overview

My task was to create a user creation form that will send a GET request to populate certain fields, and a POST request to update user submissions. The site should check for all required fields to be filled out and should alert the user that their submission has been processed.

## Tools I Used

I used SvelteKit as a framework, as this is what I am most comfortable working with.

I used tailwind-css and flowbite-svelte to create a sleek and simple form.

## GET Request

The following code was used to get data for the population of my dropdowns:

```
//GET from url on site mounting
    onMount(async () => {
        const response = await fetch(URL);
        if (response.status === 200) {
            console.log("Success");
            recordsObject = await response.json();
            //stringify JSON file and parse to splitJSON
            records = JSON.stringify(recordsObject);
            splitJSON = JSON.parse(records);
            //grab occupation list
            occupationsMap = splitJSON.occupations;
            //create states object
            states = splitJSON.states;

            //create occupations object to be read by the dropdown
            occupations = occupationsMap.map( occupationsMap => {
                return {
                    value:occupationsMap,
                    name:occupationsMap
                }
            })

        } else {
            throw new Error(response.status);
        }
    });
```

I used an onMount function to read the data as soon as the site is loaded. Once I have fetched the URL, I stringify the JSON data and parse it as to get both the occupations list and the states objects as their own. Then, I map the occupations list as an object in order to display it with named values in my dropdown.

## POST Request

The following code was used to post json data that has been chosen by the user:

```
async function doPost () {

        visible = true;

        const res = await fetch(URL, {
            method: 'POST',
            body: JSON.stringify({
                name,
                email,
                password,
                occupation,
                state
            })
        })

        //string being sent to the JSON
        const json = await res.json()
        result = JSON.stringify(json)

        console.log(result)
    }
```

The format of the input is specified by the body tag.

All fields of the form have a "required" tag so as not to post until all fields have been filled out.
