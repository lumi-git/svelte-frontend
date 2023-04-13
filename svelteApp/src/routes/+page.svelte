<script>
    import { onMount } from "svelte";

    //on init notre variable datas qui va contenir la réponse de l'API
    let datas;
    
    onMount(async () => {
        //avec fetch, on va chercher la réponse de l'API python, qui est sur le port 5000
      fetch("http://127.0.0.1:5000")
      .then(response => response.json())
      .then(data => {
            console.log(data);
            //quand on a la réponse, on stock dedans
            datas = data;
      }).catch(error => {
        console.log(error);
      });
    });
    
    //ici on créer la fonction qui va envoyer les données au serveur python (API)
    let data_to_send = {};
    function sendInfo(){
        
        //send the data to the API
        
        //on récupère les données du formulaire
        data_to_send.Username = document.querySelector("input[name=Username]").value;
        data_to_send.Password = document.querySelector("input[name=Password]").value;


        console.log(data_to_send);
        fetch("http://127.0.0.1:5000/sendInfo", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(data_to_send),
        }).then(response => response.json())
        .then(data => {
            console.log(data);
            //quand on a la réponse, on stock dedans
            datas = data;
      })
    
    }
    </script>

<h1 class = "text-center text-5xl"> Médicaments </h1>


<p>Recu de l'API python : {{datas}} </p>


<h1 class = "text-center text-5xl"> LogIn </h1>

<!-- ici on créer le formulaire qui permet d'entrer puis envoyer ses infos au server -->

<form on:submit={sendInfo} class = "flex">

    <div class= "block">
        <label>Username</label>
        <input name="Username">
    </div>
    <div>
        <label>Password</label>
        <input name="Password">
    </div>
    
    <input type="submit" name="next" value="SEND">
  </form>