<template>
  <div id="app" class="content">
    <header>
      <nav class="navbar">
        <div class="logo">
          <span class="logo-text">AndiApp</span>
        </div>
        <input type="checkbox" id="navbar-toggle-checkbox" class="navbar-toggle-checkbox">
        <label for="navbar-toggle-checkbox" class="navbar-toggle-label">
          <span class="navbar-toggle-icon"></span>
        </label>
        <ul class="nav-list">
          <li><router-link to="/infocountry" class="nav-button" exact-active-class="active">InfoCountry</router-link></li>
          <li><router-link to="/" class="nav-button" exact-active-class="active">Weather</router-link></li>
          <li><router-link to="/location" class="nav-button" exact-active-class="active">Location</router-link></li>
          <li><router-link to="/stopwatch" class="nav-button" exact-active-class="active">Stopwatch</router-link></li>
          <li><router-link to="/tictactoe" class="nav-button" exact-active-class="active">Tic Tac Toe</router-link></li>
          <li><router-link to="/photo" class="nav-button" exact-active-class="active">Photo</router-link></li>
        </ul>
      </nav>
    </header>
    <router-view></router-view>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background-color: #333;
  z-index: 100;
  display: flex;
  align-items: center;
  height: 60px;
  padding: 0 20px;
}

.logo {
  margin-right: auto;
}

.logo-text {
  font-family: 'Poppins', sans-serif;
  font-size: 24px;
  color: #fff;
  font-weight: bold;
  margin-left: 10px;
}

.navbar-toggle-checkbox {
  display: none;
}

.navbar-toggle-label {
  display: none;
  cursor: pointer;
  position: relative;
  padding: 10px;
}

.navbar-toggle-icon {
  display: block;
  width: 25px;
  height: 3px;
  background-color: #fff;
  position: relative;
  transition: transform 0.3s ease;
}

.navbar-toggle-icon:before,
.navbar-toggle-icon:after {
  content: '';
  width: 100%;
  height: 100%;
  background-color: #fff;
  position: absolute;
  transition: transform 0.3s ease;
}

.navbar-toggle-icon:before {
  top: -8px;
}

.navbar-toggle-icon:after {
  bottom: -8px;
}

.nav-list {
  display: flex;
  list-style: none;
  padding: 0;
  margin: 0;
  justify-content: flex-end;
  align-items: center;
}

.nav-button {
  display: inline-block;
  position: relative;
  padding: 10px 20px;
  background-color: #333;
  color: #fff;
  text-decoration: none;
  border-radius: 15px;
  transition: background-color 0.3s ease;
  border: none;
  font-family: 'Poppins', sans-serif;
  font-size: 16px;
  margin: 0 1px;
  overflow: hidden;
}

.nav-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ff6600;
  transform: scale(1.2);
  z-index: -1;
  opacity: 0;
  transition: transform 0.3s ease, opacity 0.3s ease;
}

.nav-button:hover {
  background-color: #ff6600;
}

@media (max-width: 768px) {
  .logo {
    margin-right: 0;
  }

  .nav-list {
    display: none;
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    background-color: #333;
    padding: 10px;
  }

  .nav-button {
    display: block;
    padding: 8px 16px;
    font-size: 14px;
  }

  .nav-button::before {
    display: none;
  }

  .navbar-toggle-checkbox {
    display: block;
    position: absolute;
    top: 12px;
    right: 20px;
    opacity: 0;
    height: 0;
    width: 0;
  }

  .navbar-toggle-label {
    display: block;
  }

  .navbar-toggle-icon:before {
    transform: translateY(-8px);
  }

  .navbar-toggle-icon:after {
    transform: translateY(8px);
  }

  .navbar-toggle-checkbox:checked ~ .nav-list {
    display: flex;
    flex-direction: column;
  }

  .navbar-toggle-checkbox:checked ~ .navbar-toggle-label .navbar-toggle-icon {
    transform: rotate(45deg);
    background-color: transparent;
  }

  .navbar-toggle-checkbox:checked ~ .navbar-toggle-label .navbar-toggle-icon:before {
    transform: rotate(90deg) translateX(-6px);
  }

  .navbar-toggle-checkbox:checked ~ .navbar-toggle-label .navbar-toggle-icon:after {
    transform: rotate(90deg) translateX(6px);
  }
}

.content {
  padding-top: 60px;
  margin-bottom: 20px;
}

.nav-button.active {
  transition: transform 0.3s ease, opacity 0.3s ease;
  color: orange;
}

.nav-button.active:hover {
  color: white;
}

</style>
