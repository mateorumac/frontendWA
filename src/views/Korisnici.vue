<template>
  <div class="container">
    <div class="left-column">
      <h2 class="headline">We Value Your Opinion!</h2>
      <p class="homepage">
        <i>
          We greatly appreciate the feedback from our customers <br>
          which helps us grow and improve our services. <br>
          If you have had any experience with our salon, please <br>
          leave your feedback and experience to help future <br>
          clients choose our salon.
        </i>
      </p>
      <img src="kor1.jpg" class="responsive-img" />
    </div>
    
    <div class="right-column">
      <div class="reviews-section">
        <div v-if="reviews.length" class="review-list">
          <div v-for="rev in reviews" :key="rev._id" class="review-item">
            <div class="review-header">
              <p class="review-author">{{ rev.reviewerName }}</p>
              <div class="review-rating">
                <span v-for="n in 5" :key="n" :class="{'filled': n <= rev.rating}">&star;</span>
              </div>
            </div>
            <p class="review-text">{{ rev.review }}</p>
          </div>
        </div>

        <div v-if="isAuthenticated()" class="feedback-form">
          <h2>Leave Your Feedback!</h2>
          <form>
            <div class="form-group">
              <label for="username">Your Name</label>
              <input type="text" id="username" v-model="username" />
              <label for="review">Your Feedback</label>
              <textarea id="review" v-model="review"></textarea>
            </div>
            <div class="form-group">
              <div class="star-rating">
                <input type="radio" id="5-stars" name="rating" value="5" v-model="rating" />
                <label for="5-stars" class="star">&bigstar;</label>
                <input type="radio" id="4-stars" name="rating" value="4" v-model="rating" />
                <label for="4-stars" class="star">&bigstar;</label>
                <input type="radio" id="3-stars" name="rating" value="3" v-model="rating" />
                <label for="3-stars" class="star">&bigstar;</label>
                <input type="radio" id="2-stars" name="rating" value="2" v-model="rating" />
                <label for="2-stars" class="star">&bigstar;</label>
                <input type="radio" id="1-star" name="rating" value="1" v-model="rating" />
                <label for="1-star" class="star">&bigstar;</label>
              </div>
            </div>
            <button type="button" @click="submitReview()">Submit</button>
          </form>
        </div>
        
        <div v-else class="login-prompt">
          <h2>Please log in to leave your feedback!</h2>
          <button class="redirect" @click="redirectToPage()">Log In</button>
        </div>
      </div>
    </div>
    
    <div v-if="showSuccessPopup" class="success-popup">
      Your feedback has been recorded!
      <button @click="closePopup">Close</button>
    </div>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;600&display=swap');

body {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.container {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  max-width: 1400px;
  margin: 0 auto;
  padding: 20px;
  padding-bottom: 35px;
  opacity: 0;
  animation: fadeIn 1s forwards;
}

.left-column {
  flex: 2;
  margin-left: 0;
  padding-right: 50px;
  text-align: center;
  transform: translateX(-20px);
  animation: slideInLeft 0.5s ease-out forwards;
}

.right-column {
  flex: 1;
  padding-left: 80px;
  text-align: center;
  transform: translateX(20px);
  animation: slideInRight 0.5s ease-out forwards;
}

.headline {
  margin-bottom: 10px;
  margin-top: 20px;
  opacity: 0;
  animation: fadeInUp 1s ease-out forwards;
}

.homepage {
  font-family: 'Open Sans', sans-serif;
  margin-bottom: 20px;
}

.responsive-img {
  max-width: 75%;
  height: auto;
  display: block;
  margin: 20px auto;
  border-radius: 10px;
  opacity: 0;
  animation: fadeIn 1.5s forwards;
}

.form-group {
  margin-bottom: 1rem;
}

label {
  display: block;
  font-weight: bold;
  font-family: 'Open Sans', sans-serif;
  margin-bottom: 10px;
  opacity: 0;
  animation: fadeInUp 1s ease-out forwards;
}

input[type="text"],
textarea {
  width: 100%;
  padding: 10px;
  font-family: 'Open Sans', sans-serif;
  margin-bottom: 10px;
  transition: border-color 0.3s ease;
}

input[type="text"]:focus,
textarea:focus {
  border-color: #FFB6C1;
}

.redirect,
button[type="button"] {
  font-family: 'Open Sans', sans-serif;
  background-color: #FFB6C1;
  color: black;
  padding: 10px 20px;
  font-size: 16px;
  border-radius: 5px;
  border: none;
  cursor: pointer;
  display: inline-block;
  margin-top: 10px;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.redirect:hover,
button[type="button"]:hover {
  background-color: #ff99a8;
  transform: scale(1.05);
}

.success-popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0,0,0,0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  font-size: 18px;
  color: white;
  z-index: 1000;
  opacity: 0;
  animation: fadeIn 0.5s ease-out forwards;
}

.success-popup button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
  background-color: #FFB6C1;
  color: black;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.success-popup button:hover {
  background-color: #ff99a8;
  transform: scale(1.05);
}

.review-list {
  margin-top: 20px;
}

.review-item {
  background-color: #f9f9f9;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 15px;
  margin-bottom: 10px;
  text-align: left;
}

.review-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.review-author {
  font-weight: bold;
  font-size: 1.2em;
}

.review-rating {
  display: flex;
}

.review-rating span {
  color: #ddd;
  font-size: 1.5em;
}

.review-rating .filled {
  color: #ffb400;
}

.review-text {
  margin-top: 10px;
  font-size: 1em;
}

.star-rating {
  display: flex;
  flex-direction: row-reverse;
  font-size: 1.5em;
  justify-content: center;
  text-align: center;
}

.star-rating input {
  display: none;
}

.star-rating label {
  color: rgb(128, 128, 128);
  cursor: pointer;
  transition: color 0.3s ease;
}

.star-rating :checked ~ label {
  color: rgb(255, 212, 59);
}

.star-rating :hover ~ label {
  color: rgb(250, 176, 5);
}

@media (min-width: 769px) and (max-width: 1024px) {
  .container {
    flex-direction: column;
    padding: 20px;
  }

  .left-column, .right-column {
    width: 100%;
    padding: 10px;
    text-align: center;
  }

  .right-column {
    padding-left: 0;
  }

  .responsive-img {
    max-width: 100%;
    margin: 20px auto;
  }
}

@media (min-width: 1025px) and (max-width: 1440px) {
  .container {
    flex-direction: row;
    justify-content: space-between;
    padding: 20px;
  }

  .left-column {
    flex: 1;
    padding-right: 40px;
  }

  .right-column {
    flex: 1;
    padding-left: 40px;
  }

  .responsive-img {
    max-width: 90%;
    height: auto;
    margin: 0 auto;
  }
}

@media (max-width: 768px) {
  .container {
    flex-direction: column;
    opacity: 1;
    animation: none;
  }

  .headline,
  .responsive-img,
  .login-prompt,
  label,
  input[type="text"],
  textarea {
    animation: none;
    opacity: 1;
  }

  .left-column, .right-column {
    padding: 10px;
    width: 100%;
  }

  .responsive-img {
    max-width: 100%;
  }

  .login-prompt {
    text-align: center;
    margin-top: 40px;
  }

  .redirect {
    display: inline-block;
    margin-top: 20px;
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slideInLeft {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes slideInRight {
  from {
    opacity: 0;
    transform: translateX(20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}
</style>

<script>
import { getUserRole, isAuthenticated } from '../router/helpers';

export default {
  name: 'ReviewForm',
  data() {
    return {
      rating: null,
      review: '',
      username: '',
      showSuccessPopup: false,
      reviews: [],
      role: getUserRole()
    };
  },
  mounted() {
    this.fetchReviews(); 
  },
  methods: {
    isAuthenticated,
    closePopup() {
      this.showSuccessPopup = false;
    },
    redirectToPage() {
      window.location.href = '/login'; 
    },
    async submitReview() {
      const reviewData = {
        review: this.review,
        rating: this.rating,
        reviewerName: this.username
      };

      try {
        const response = await fetch("https://wa-backend4.onrender.com/api/review/review", {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(reviewData)
        });

        if (response.ok) {
          const responseData = await response.json();
          this.showSuccessPopup = true;
          this.username = '';
          this.review = '';
          this.rating = 0;

          if (responseData.review) {
            this.reviews.push(responseData.review); 
          }
        } else {
          console.error('Error submitting review', await response.text());
        }
      } catch (error) {
        console.error('Error submitting review', error);
      }
    },

    async fetchReviews() {
      try {
        const response = await fetch("https://wa-backend4.onrender.com/api/review/svi");
        if (response.ok) {
          this.reviews = await response.json();
        } else {
          console.error("Failed to fetch reviews");
        }
      } catch (error) {
        console.error('Error fetching the reviews:', error);
      }
    }
  }
};
</script>

