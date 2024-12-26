<template>
	<header class="header">
		<div class="logo-container">
			<img src="@/assets/logo.png" alt="Hospital Logo" class="logo" />
			<h1 class="hospital-name">Haleshi Clinic</h1>
		</div>
		<nav class="navbar">
			<ul>
				<li><router-link to="/">Home</router-link></li>
				<li><router-link to="/services">Services</router-link></li>
				<li><router-link to="/doctors">Doctors</router-link></li>
				<li><router-link to="/contact">Contact</router-link></li>
			</ul>
		</nav>
		<div class="contact-info">
			<span>Call us: <strong>9800000</strong></span>
		</div>
	</header>
</template>

<script>
export default {
	name: "HospitalHeader",
};
</script>

<style scoped>
/* General styles for header */
.header {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	display: flex;
	justify-content: space-between;
	align-items: center;
	background-color: #2c3e50;
	padding: 20px;
	color: white;
	z-index: 1000;
}

.logo-container {
	display: flex;
	align-items: center;
}

.logo {
	width: 50px;
	height: 50px;
	margin-right: 10px;
}

.hospital-name {
	font-size: 24px;
	font-weight: bold;
}

.navbar ul {
	list-style: none;
	display: flex;
	gap: 20px;
}

.navbar a {
	text-decoration: none;
	color: white;
	font-size: 18px;
	transition: color 0.3s;
}

.navbar a:hover {
	color: #3498db;
}

.contact-info {
	font-size: 16px;
	font-weight: bold;
}

.contact-info strong {
	font-size: 18px;
	color: #f39c12;
}

/* Mobile responsiveness */
@media (max-width: 768px) {
	.header {
		flex-direction: column;
		padding: 15px;
	}

	.logo-container {
		margin-bottom: 10px;
		justify-content: center;
	}

	.navbar ul {
		flex-direction: column;
		gap: 15px;
		margin-top: 15px;
	}

	.navbar a {
		font-size: 16px;
	}

	.contact-info {
		margin-top: 15px;
		text-align: center;
	}
}

/* For smaller mobile screens */
@media (max-width: 480px) {
	.hospital-name {
		font-size: 20px;
	}

	.logo {
		width: 40px;
		height: 40px;
	}

	.navbar a {
		font-size: 14px;
	}

	.contact-info strong {
		font-size: 16px;
	}
}
</style>
