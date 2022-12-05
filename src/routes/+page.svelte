<script>
    import { onMount } from "svelte";
    import {Label, Select, Helper, Input, Button} from "flowbite-svelte";
    import {Alert} from "flowbite-svelte";

    //controls completion alert visibility
    let visible = false;

    const URL =
        "https://frontend-take-home.fetchrewards.com/form";
    let recordsObject = [];
    let records;

    let splitJSON = [];
    let occupationsMap;
    let occupations;
    let states;

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

    let name;
    let email;
    let password;
    let occupation;
    let state;
    let result = null;

    //POST to url
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

</script>

<h1>Create Your Account</h1>

<!-- user form -->
<form method='POST' on:submit|preventDefault={doPost}>
    <div class="grid gap-6 mb-6 md:grid-cols-2">
        <div>
            <Label for="name" class="mb-2">Name</Label>
            <Input bind:value={name} type="text" id="name" placeholder="John" required />
        </div>

        <div>
            <Label for="email" class="mb-2">E-mail</Label>
            <Input bind:value={email} type="text" id="email" placeholder="youremail@here.com" required />
        </div>

        <div>
            <Label for="password" class="mb-2">Password</Label>
            <Input bind:value={password} type="text" id="password" placeholder="password" required />
        </div>
        <div>
            <Label for="occupation" class="mb-2">Occupation</Label>
            <Select bind:value={occupation} id="occupation" underline class="mt-2" items={occupations} required />
        </div>

        <div>
            <Label for="states" class="mb-2">State</Label>
            <Select bind:value={state} id="occupation" underline class="mt-2" items={states} required />
        </div>

        <div>
            <Button value = "Submit" type="submit">Submit</Button>
        </div>
    </div>
</form>

{#if visible}
    <Alert>Your account has been created!</Alert>
{/if}

<!-- style -->
<style>
    h1{
        padding: 30px;
        font-size: 30px;
    }

    form{
        padding: 30px;
    }
</style>