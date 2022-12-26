<script>
  import { HtmlTag, prevent_default } from "svelte/internal";

  const genForm = document.getElementById('gen-form');
  const genBtn = document.getElementById('gen-btn');
  const copyBtn = document.getElementById('copy-btn');
  const genContent = document.getElementById('text-content');
  // const genValue = document.getElementById('gen_count');

  //initialize variables--> count,type
  let count = 0;
  let optionSelected = "type";
  let tempOption = "";
  let tempCount = 0;

  // on click generate text 
  function onClick(e){
    // console.log(count,optionSelected);

    // validate values
    validateValues();

    // api for dummy text generation
    let url = `https://baconipsum.com/api/?type=meat-and-filler&${optionSelected}=${count}&start-with-lorem=1`;
    fetchContent(url); // fetch this url 
  }

  // function for validating the count and option
  function validateValues(){
    // if select word
    if(optionSelected === "words"){
      tempCount = count; // store count and option in temp value
      tempOption = optionSelected;
      // optionSelected = "paras";
      // count = 100;

      // if word count is more than 2000 --> we can access only 200 words
      if(tempCount > 2000){
        invalidInput();
        tempCount = 2000;  // print only 2000 words
      }
      else if(tempCount<1 || isNaN(tempCount)){
        invalidInput();
        tempCount = 1;  // if text box is empty or count < 1---> set count to 1
      }
    }
    else{ // if option is sentences or paras
      tempCount = 0;  
      if(count > 100){  // we can generate only 100 sentences or paras
        invalidInput();
        count = 100;  // print only 100 paras or sentences
        
      }
      else if(count < 1 || isNaN(count)){
        invalidInput();
        count = 1;  // if text box empty or count < 1 --> then print only one sentence or paragraph
      }
    }
  }

  // function --> if invalid value detected
  function invalidInput(){
    // genForm.gen_count.style.borderColor = "red";
    setTimeout(()=>{
      //---------------
    },2000);
  }

  // fetch the displayed dummy text
  async function fetchContent(url){
    let response = await fetch(url);
    if(response.status === 200){
      let data = await response.json();
      // console.log(data);
      displayGenContent(data);
    }
    else{
      alert("An error Occured");
    }
  }

  // this is the dummy text which we change using count and option selected...
  let texts = "";

  // display the generated random text
  function displayGenContent(data){
    // console.log(data);
    
    //if word is selected 
    if(tempOption === "words"){
      // console.log();
      // tempOption = "";
      texts = data.join();  // join all paragraphs 
      // console.log(texts); ---> provide full 100 paras text
      if(tempCount <= texts.length){
        let textArray = texts.split(" "); // split this text with space 
        // console.log(textArray);
        let selectedText = textArray.splice(0,tempCount).join(" "); // select text according to count 
        // textContent.innerHTML = selectedText + ".";
        // console.log(selectedText);
        texts = selectedText;  // reset this selected text value to text
      }
      return ;

    }
    else{ // if sentence or para selected
      
      texts = data.join("<br><br>");  // gives a line break between paras
      // console.log(texts);
      
    }
  }

  // function for copy button --> to copy the text
  function copyToClipboard(){

    navigator.clipboard.writeText(texts);
  }

  
</script>

<main>
  <body class="body-class">
    <nav class="nav-bar">
      <span>
        <!-- <img src="src/Images/logo.png" alt=""> -->
        <h1 class="logo-text" style="color: #0066ff">Lorem Ipsum</h1>
        <p>A Dummy Text Generator</p>
      </span>
    </nav>
    <section class="section1" style="border: 20px;">
      <div class="home-body"></div>
      <div class="side-text">
        <h1>Lorem ipsum</h1><h4>is placeholder text commonly used in the graphic, print, and publishing industries for previewing layouts and visual mockups.</h4>
      </div>
      <div class="side-below">
        <h2>Create.</h2>
        <h2>Copy..</h2>
        <h2>Paste..!</h2>
      </div>
    </section>

    <section class="section2">
      <!-- <div class="head-section">
        <h1 style="color: #0066ff">Try our Dummy Text Generator</h1>
        <hr>
        <p>
          The classic latin passage that just never gets old, enjoy as much (or
          as little) lorem ipsum as you can handle with our easy to use filler
          text generator.
        </p>
      </div> -->

      <div class="input-div">
      <form id="gen-form" on:submit|preventDefault={onClick}>
        
          <label for="gen_count">Enter Value</label>
          <input name="gen_count" type="number" class="form-control" min="1" max="100" bind:value={count}>
        
        
          <label for="gen_option">Select Type</label>
          <select name="gen_option" class="form-control" bind:value={optionSelected}>
            <option value="words">Words</option>
            <option value="sentences">Sentences</option>
            <option value="paras">Paragraphs</option>
          </select>
        
          <button type="submit" id="gen-btn" class="btn">Generate</button>
      </form>
      </div>
      <div class="text-body">
        <div id="text-content">{@html texts}</div>
      </div>
      <div class="button-bar">
        <button type="button" id="copy-btn" class="btn" on:click={copyToClipboard}>Copy Text</button>
      </div>
      
    </section>
  </body>
</main>

<style>
@import url("https://fonts.googleapis.com/css2?family=Scada:wght@400;700&display=swap");

*{
  padding: 0;
  margin: 0;
}

/* .body-class {
  height: 100vh;
  width: 100vw;
  font-family: "Scada", sans-serif !important;
} */

.nav-bar {
  width: 100%;
  padding: 1em 1.5em;
  box-sizing: border-box;
  /* display:block; */
}

/* .nav-bar img{
  height: 60px;
  border-radius: 10px;
  
} */

.nav-bar p{
  font-size: 15px;
  padding-left: 70px;
  font-style: italic;
  color: #293548;
  
}

.logo-text{
  display: inline; /* move this text to right siide of logo*/
  font-family:serif;
  font-size: 48px;
  /* font-style: italic; */
  /* font-weight: 100px; */
  /* line-height: 60px; */
}


section {
  margin: 1em;
  border-radius: 10px;
}

input {
  background: #ffffff;
  border: 1px solid #d0d5dd;
  box-shadow: 0px 1px 2px rgba(16, 24, 40, 0.05);
  border-radius: 8px;
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 10px 14px;
  width: 320px;
}

select {
  background: #ffffff;
  border: 1px solid #d0d5dd;
  box-shadow: 0px 1px 2px rgba(16, 24, 40, 0.05);
  border-radius: 8px;
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 10px 14px;
  width: 320px;
}

.home-body {
  background-image: url(src/Images/20827476_Tiny\ characters\ sitting\ on\ laptop\ with\ lorem\ ipsum\ title.jpg);
  height: 80vh;
  background-color: white;
  background-size:contain;
  background-repeat: no-repeat;
  border: 20px;
  margin: auto;
  border-radius: 20px;
  box-shadow: 2px 5px 5px rgba(16, 24, 40, 0.05);

}

.side-text{
  /* border: 4px solid red; */
  position:absolute;
  height: 246px;
  width: 579px;
  top: 200px;
  right: 150px;

}

.side-text h1{
  font-size: 100px;
  color: rgb(27, 26, 26);
  /* bottom right color  */
  text-shadow: 2px 2px #72afec; 
}

.side-text h4{
  font-size: 20px;
  font-family:monospace;
  color: rgb(69, 67, 67);
}

.side-below{
  position: absolute;
  top: 400px;
  right: 500px;
}

.side-below h2{
  font-size: 70px;
  color: rgb(249, 169, 20);
  
}

/* .head-section {
  margin: auto;
  width: 60%;
  padding: 1em;
}

.head-section h1{
  font-size: 48px;
  text-shadow: 2px 2px #b9d3fd;
}

.head-section p{
  width: 610px;
  padding: 0.8em 0;
  font-size: 20px;
  font-family:monospace;
  font-weight: bold;
  color: rgb(69, 67, 67);

} */

.input-div {
  /* border:2px solid rgb(98, 98, 255); */
  box-shadow: 2px 2px rgb(64, 63, 63);
  /* box-shadow: 0px 0px 0px 1px rgba(0, 0, 0, 0.05), 0px 15px 30px rgba(0, 0, 0, 0.12); */
  background-color: #aeccea;
  border-radius: 20px;
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin: 1em auto;
  width: 60%;
  margin-bottom: 50px;
  padding: 1em;
}

.input-div form{
  display: flex;
  flex-direction: row;
  
}

.input-div span label {
  font-family: sans-serif;
  font-size: 1.1em;
  margin-bottom: 1em;
}

.text-body {
  background-color: #c0d7f9;
  /* height: 60vh; */
  width: 70%;
  /* border-radius: 20px; */
  margin: auto;
  min-height: 500px;
  box-shadow: 0 0 13px 0 rgb(106, 103, 103);

}

.text-body #text-content{
  /* width:100%; */
  max-height: 500px;
  overflow-y:scroll;  /** if text over flow in line then it takes to next line */
  padding: 2.8rem;
  font-size: 1.2rem;
  color: #36312d;
  font-weight: 800;
  line-height: 1.5;
  
}

/* css for the scroll bar  */
#text-content::-webkit-scrollbar{
  width: 10px;
}

#text-content::-webkit-scrollbar-track{
  box-shadow: inset 0 0 6px rgb(113, 103, 103);
}

#text-content::-webkit-scrollbar-thumb{
  background-color: #195ecd;
  border-radius: 10px;
}

button {
  width: 116px;
  height: 36px;
  font-weight: 400;
  font-size: 20px;
  line-height: 25px;
  background: #0066ff;
  border-radius: 10px;
  color: white;
  border: none;
}

.button-bar {
  width: 70%;
  margin: auto;
  display: flex;
  justify-content: end;
}

.button-bar button {
  margin: 1em;
}

</style>
