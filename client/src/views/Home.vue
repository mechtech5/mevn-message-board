<template>
  <div>
  	<section class="container">
  		<div class="modal is-active" v-show="isActive">
  		  <div class="modal-background"></div>
  		  <div class="modal-card">
  		    <header class="modal-card-head">
  		      <p class="modal-card-title">Post new message</p>
  		      <button class="delete" aria-label="close" @click="hideModal()"></button>
  		    </header>
  		    <form @submit.prevent="addMessage">
  		    	<section class="modal-card-body">
  		      	<div class="field">
  		      	  <label class="label">Username *</label>
  		      	  <div class="control has-icons-left has-icons-right">
  		      	    <input v-model="message.username" class="input is-success" id="username" type="text" required>
  		      	    <span class="icon is-small is-left">
  		      	      <i class="fa fa-user"></i>
  		      	    </span>
  		      	    <span class="icon is-small is-right">
  		      	      <i class="fa fa-check"></i>
  		      	    </span>
  		      	  </div>
  		      	  <!-- <p class="help is-success">This username is available</p> -->
  		      	</div>
  		      	<div class="field">
  		      	  <label class="label">Subject *</label>
  		      	  <div class="control">
  		      	    <input v-model="message.subject" class="input" id="subject" type="text" placeholder="Enter a subject..." required>
  		      	  </div>
  		      	</div>
  		      	<div class="field">
  		      	  <label class="label">Message *</label>
  		      	  <div class="control">
  		      	    <textarea v-model="message.message" class="textarea" id="message" placeholder="Write a message..." required></textarea>
  		      	  </div>
  		      	</div>
  		      	<div class="field">
  		      	  <label class="label">Image URL</label>
  		      	  <div class="control">
  		      	    <input v-model="message.imageURL" class="input" id="imageURL" type="url" placeholder="Put an image URL...">
  		      	  </div>
  		      	</div>
  		    	</section>
	  		    <footer class="modal-card-foot">
	  		      <button type="submit" class="button is-success">Save changes</button>
	  		      <button class="button" @click="hideModal()">Cancel</button>
	  		    </footer>
  		    </form>
  		  </div>
  		</div>
			<div class="columns">
				<div class="column is-3">
					<a class="button is-primary is-block is-alt is-large" href="#" @click.prevent="showModal()">New Post</a>
					<aside class="menu">
						<p class="menu-label">
							Tags
						</p>
						<ul class="menu-list">
							<li><span class="tag is-primary is-medium ">Dashboard</span></li>
							<li><span class="tag is-link is-medium ">Customers</span></li>
							<li><span class="tag is-light is-danger is-medium ">Authentication</span></li>
							<li><span class="tag is-dark is-medium ">Payments</span></li>
							<li><span class="tag is-success is-medium ">Transfers</span></li>
							<li><span class="tag is-warning is-medium ">Balance</span></li>
							<li><span class="tag is-medium ">Question</span></li>
						</ul>
					</aside>
				</div>
				<div class="column is-9">
					<div class="box content">
						<article class="post" v-for="message in messages" :key="message._id">
							<h4>{{message.subject}}</h4>
							<div class="media">
								<div class="media-left">
									<p class="image is-32x32">
										<!-- <img src="http://bulma.io/images/placeholders/128x128.png"> -->
										<img :src="message.imageURL">
									</p>
								</div>
								<div class="media-content">
									<div class="content">
										<p>
											{{message.message}} <br />
											<a href="#">@{{message.username}}</a> {{message.created}} &nbsp;
											<span class="tag">Question</span>
										</p>
									</div>
								</div>
								<div class="media-right">
									<span class="has-text-grey-light"><i class="fa fa-comments"></i> 1</span>
								</div>
							</div>
						</article>
					</div>
				</div>
			</div>
  	</section>
  </div>
</template>

<script>
const API_URL = 'http://localhost:1234/messages';

export default {
  name: 'home',
  data: () => ({
		messages: [],
  	message: {
  		username: 'Anonymous',
  		subject: '',
  		message: '',
  		imageURL: '',
  	},
  	isActive: false,
  }),
  mounted() {
  	fetch(API_URL).then(response => response.json()).then(result => {
  		// console.log(result);
  		this.messages = result;
  	});
  },
  methods: {
  	showModal() {
  		this.isActive = true;
  	},
  	hideModal() {
  		this.isActive = false;
  	},
  	addMessage() {
  		console.log(this.message);
  		fetch(API_URL, {
  			method: 'POST',
  			body: JSON.stringify(this.message),
  			headers: {
  				'content-type': 'application/json' 
  			}
  		}).then(response => response.json()).then(result => {
  			// console.log(result);
  			if (result.details) {
  				//there was an error...
  				const error = result.details.map(detail => detail.message).join(' ')
  			} else {
  				this.messages.push(result);
  			}
  		});
  	}
  },
};
</script>

<style>
	html,body {
	  font-family: 'Open Sans', serif;
	  background: #F2F6FA;
	}
	footer {
	  background-color: #F2F6FA !important;
	}
	.topNav {
	  border-top: 5px solid #3498DB;
	}
	.topNav .container {
	  border-bottom: 1px solid #E6EAEE;
	}
	.container .columns {
	  margin: 3rem 0;
	}
	.navbar-menu .navbar-item {
	  padding: 0 2rem;
	}
	aside.menu {
	  padding-top: 3rem;
	}
	aside.menu .menu-list {
	  line-height: 1.5;
	}
	aside.menu .menu-label {
	  padding-left: 10px;
	  font-weight: 700;
	}
	.button.is-primary.is-alt {
	  background: #00c6ff;
	  background: -webkit-linear-gradient(to bottom, #0072ff, #00c6ff);
	  background: linear-gradient(to bottom, #0072ff, #00c6ff);
	  font-weight: 700;
	  font-size: 14px;
	  height: 3rem;
	  line-height: 2.8;
	}
	.media-left img {
	  border-radius: 50%;
	}
	.media-content p {
	  font-size: 14px;
	  line-height: 2.3;
	  font-weight: 700;
	  color: #8F99A3;
	}
	article.post {
	  margin: 1rem;
	  padding-bottom: 1rem;
	  border-bottom: 1px solid #E6EAEE;
	}
	article.post:last-child {
	  padding-bottom: 0;
	  border-bottom: none;
	}
	.menu-list li{
	  padding: 5px;
	}
</style>
