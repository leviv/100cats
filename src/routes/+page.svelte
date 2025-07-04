<script lang="ts">
  import data from "$lib/data.json";
  let catPicIndex = 0
  let matchedCat: any = null
  let answers = {
    q1: null,
    q2: null,
    q3: null,
    q4: null,
    q5: null
  }
  
  $: allAnswered = answers.q1 !== null && answers.q2 !== null && answers.q3 !== null && answers.q4 !== null && answers.q5 !== null
  
  function changeImage() {
    catPicIndex++
    if (catPicIndex >= data.length) {
      catPicIndex = 0
    }
    setTimeout(changeImage, 300)
  }

  function handleSubmit(event: Event) {
    event.preventDefault()
    const formData = new FormData(event.target as HTMLFormElement)
    
    // Get user's answers
    const userAnswers = {
      q1: formData.get('q1') === 'true',
      q2: formData.get('q2') === 'true',
      q3: formData.get('q3') === 'true',
      q4: formData.get('q4') === 'true',
      q5: formData.get('q5') === 'true'
    }
    
    // Find the cat with the closest match
    let bestMatch = data[0]
    let bestScore = 0
    
    for (const cat of data) {
      let score = 0
      if (cat.q1 === userAnswers.q1) score++
      if (cat.q2 === userAnswers.q2) score++
      if (cat.q3 === userAnswers.q3) score++
      if (cat.q4 === userAnswers.q4) score++
      if (cat.q5 === userAnswers.q5) score++
      
      if (score > bestScore) {
        bestScore = score
        bestMatch = cat
      }
    }
    
    matchedCat = bestMatch
    
    // Smooth scroll to bottom of page
    setTimeout(() => {
      window.scrollTo({
        top: document.body.scrollHeight,
        behavior: 'smooth'
      })
    }, 100)
  }

  setTimeout(changeImage, 300)
</script>

<div class="hero">
  <h1>Welcome to the 100cats Fursonality test</h1>
  <img src={data[catPicIndex].image} alt={data[catPicIndex].name} class="heroImage" />

  <h2>Take the test and find your cat!</h2>
</div>

<form on:submit={handleSubmit}>  
  <div class="question">
    <p><strong>Q1: </strong> Would you rather:</p>
    <div class="option">
      <input type="radio" id="q1-true" name="q1" value="true" bind:group={answers.q1}>
      <label for="q1-true">
        Stay in the United States and never be able to leave for the rest of your life
      </label>
    </div>
    <div class="option">
      <input type="radio" id="q1-false" name="q1" value="false" bind:group={answers.q1}>
      <label for="q1-false">Leave the United States and never be able to return</label>
    </div>
  </div>

  <div class="question">
    <p><strong>Q2: </strong> Is your daily screentime: </p>
    <div class="option">
      <input type="radio" id="q2-true" name="q2" value="true" bind:group={answers.q2}>
      <label for="q2-true">5+ hours</label>
    </div>
    <div class="option">
      <input type="radio" id="q2-false" name="q2" value="false" bind:group={answers.q2}>
      <label for="q2-false">Less than 5 hours</label>
    </div>
  </div>

  <div class="question">
    <p><strong>Q3: </strong> Do you prefer to make art: </p>
    <div class="option">
      <input type="radio" id="q3-true" name="q3" value="true" bind:group={answers.q3}>
      <label for="q3-true">Digitally</label>
    </div>
    <div class="option">
      <input type="radio" id="q3-false" name="q3" value="false" bind:group={answers.q3}>
      <label for="q3-false">Physically</label>
    </div>
  </div>

  <div class="question">
    <p><strong>Q4: </strong> Your significant other has been kidnapped. Riding on the way to the ransom point do you: </p>
    <div class="option">
      <input type="radio" id="q4-true" name="q4" value="true" bind:group={answers.q4}>
      <label for="q4-true">Listen to Spotify</label>
    </div>
    <div class="option">
      <input type="radio" id="q4-false" name="q4" value="false" bind:group={answers.q4}>
      <label for="q4-false">Sit in silence</label>
    </div>
  </div>

  <div class="question">
    <p><strong>Q5: </strong> Club Penguin or Poptropica </p>
    <div class="option">
      <input type="radio" id="q5-true" name="q5" value="true" bind:group={answers.q5}>
      <label for="q5-true">Club Penguin </label>
    </div>
    <div class="option">
      <input type="radio" id="q5-false" name="q5" value="false" bind:group={answers.q5}>
      <label for="q5-false">Poptropica</label>
    </div>
  </div>


  <input type="submit" value="Submit" class="submit" disabled={!allAnswered}>
</form>

{#if matchedCat}
  <div class="results">
    <h2>Your Fursonality is:</h2>
    <div class="cat-result">
      <h3>{matchedCat.name}</h3>
      <img src={matchedCat.image} alt={matchedCat.name} class="result-image" />
      <p>{matchedCat.description}</p>
    </div>
  </div>
{/if}

<footer>
<p>A <a href="https://instagram.com/100days200cats">100days200cats</a> project by <a href="https://lyuu.cc/">lucy</a> and <a href="https://leviv.cool">levi</a></p>

<p>Made with 🫶 in NYC &copy; 2025</p>
</footer>

<style>
  .hero {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  .heroImage {
    height: 400px;
  }

  form {
    max-width: 800px;
    margin: 0 auto;
  }

  .option {
    margin-bottom: 10px;
  }

  .question {
    border-bottom: 1px solid black;
  }

  .submit {
    margin: 50px auto;
    padding: 10px 20px;
    cursor: pointer;
  }

  .submit:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }

  .results {
    max-width: 760px;
    margin: 0 auto 50px auto;
    text-align: center;
    padding: 20px;
    border: 1px solid #333;
    border-radius: 10px;
    background-color: #f9f9f9;
  }

  .cat-result {
    margin-top: 20px;
  }

  .result-image {
    height: 300px;
    border-radius: 10px;
    margin-top: 10px;
  }

  footer {
    text-align: right;
    max-width: 800px;
    margin: 0 auto 40px auto;
  }
</style>