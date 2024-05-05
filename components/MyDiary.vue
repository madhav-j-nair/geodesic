<template>
  <div>
    <div>
      <span class="words1">I </span>
      <span class="img1">❤️</span>
      <span class="words2">MY DIARY</span>
    </div>
    <div>
    <span class="date-display">{{ formattedDate }}</span> 
    </div>
    <textarea v-model="Story" placeholder="Write your heartfelt entries here..." rows="15" cols="80"></textarea>
    <div class="button-container">

      <button @click="ThoughtsShared" v-bind:disabled="Story.length==0" class="primary-button">Share my thoughts</button>
      <button @click="ThoughtsDeleted" v-bind:disabled="Story.length==0" class="secondary-button">Forget it</button>
      <button class="tertiary-button" @click="ThoughtsKept" v-bind:disabled="Story.length==0">Keep it</button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import moment from 'moment' 

export default {
  setup() {
    const Story = ref('')

    const ThoughtsShared = () => {
      if (!navigator.share) {
        alert('Oops! seems like your browser does not sipport this function')
        return;
      }

      const text = Story.value; 
      const title = 'I thought that I should share the experience I had in this beautiful day with you'; 

      navigator.share({
        title,
        text,
      })
        .then(() => console.log('Diary entry shared successfully!'))
        .catch((error) => console.error('Error sharing diary entry:', error));
    
    }
    const ThoughtsKept = () => {
      const textToSave = Story.value;
      const blob = new Blob([textToSave], { type: 'text/plain' });
      const url = window.URL.createObjectURL(blob);
      const link = document.createElement('a');
      link.href = url;
      link.download = 'my_diary_entry.txt';
      link.click();
    }
    const ThoughtsDeleted = () => {
      console.log('ThoughtsDeleted! Thoughts=', Story.value)
      Story.value = ''
    }

    // Computed property for formatted date
    const formattedDate = computed(() => {
      const date = new Date()
      return moment(date).format('YYYY-MM-DD'); // Use moment for formatting
    })

    return {
      Story,
      ThoughtsShared,
      ThoughtsKept,
      ThoughtsDeleted,
      formattedDate,
    }
  }
}
</script>
  <style>


  textarea {
    width: 50%;
    min-height: 500px;
    padding: 50px;
    font-size: 16px;
    border: 10px solid #ccc;
    border-radius: 20px;
    margin-left:300px;
    background-color:ivory;
  }
  .date-display {
  display: inline-block;
  padding: 10px 15px; 
  border-radius: 20px; 
  background-color: #f5f5f5; 
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1); 
  font-family: 'Comic Sans MS', cursive;
  font-size: 18px; 
  color: #333;
  margin-left:300px; 
  margin-bottom:10px;
}
  @font-face {
  font-family: 'Lucida Handwriting';
  src: url('/static/fonts/LucidaHandwriting.ttf') format('truetype')
}
  .words1{
  margin-left: 420px;
  font-size:90px;
  font-family: 'Lucida Handwriting', cursive;
  }

  .words2{
  margin-left: 10px;
  font-size:90px;
  font-family: 'Lucida Handwriting', cursive;
  }

  .img1{
    margin-left: 10px;
    font-size:100px;
    
  }
  
  .button-container {
   
    margin-left: 500px; 
    display: flex;
    
  }
  
  .primary-button {
    background-color: #4CAF50;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
    border: none;
    border-radius: 5px;
  }
  
  .primary-button:hover {
    background-color: #45A049;
  }
  
  .secondary-button {
    background-color: red;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
    border: none;
    border-radius: 5px;
  }
  
  .secondary-button:hover {
    background-color: maroon; 
  }
  
  .tertiary-button {
    background-color: #ffc107; 
    color: white;
    padding: 15px 32px; 
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
    border: none;
    border-radius: 5px;
  }
  
  .tertiary-button:hover {
    background-color: #ffa500; 
  }
  </style>
  