<template>
  <div id="app">
    <nav>

      <div class="navbar">
        <p>TSE</p>
        <span>Eleições 2020</span>
      </div>

      <div class="navbar-img">
        <img src="./assets/tse.png" alt="TSE" />
      </div>

    </nav>

    <router-view></router-view>

  </div>
</template>

<script>
export default {
  name: "app",
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Barlow:wght@400;600;700&display=swap");

* {
  font-family: "Barlow", sans-serif;
}

body {
  background-color: #0b0727;
  padding: 0;
  margin: 0;
  font-family: "Barlow", sans-serif;
}

nav {
  display: flex;
  margin-top: 20px;
  margin-right: 20px;
  justify-content: space-between;
}

.navbar p {
  font-family: "Barlow", sans-serif;
  color: white;
  font-size: 48px;
  font-weight: 700;
  text-align: initial;

  width: 100%;

  padding: 0;
  margin: 0;

  margin-left: 30px;
}

.navbar span {
  display: block;
  width: 100%;
  text-align: start;
  margin-left: 30px;

  font-weight: 600;
  font-size: 20px;
  line-height: 29px;

  color: rgba(255, 251, 251, 0.65);
}

.nav-menu img {
  width: 30px;
  height: 30px;

  margin-right: 5px;
  padding: 7px;
}

.main-posts {
  background-color: #0b0727;

  text-align: start;
  padding: 30px;

}

.posts p:first-child {
  padding: 10px;
  font-family: Barlow;
  font-style: normal;
  font-weight: 600;
  font-size: 20px;
  line-height: 24px;
  color: #fffbfb;
}

.main-posts span {
  display: block;
  font-size: 14px;
  color: white;
  margin-bottom: 20px;
}

.main-posts label {
  font-family: Barlow;
  font-size: 14px;
  line-height: 25px;
  color: rgba(255, 251, 251, 0.65);
}

.routerlink {
  color: rgb(36, 34, 34);
  font-size: 10px;
  background: #ffffff;
  text-decoration: none;
  font-weight: 600;

  padding: 5px;

  box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.25);
  border-radius: 5px;
}

.routerlink:hover {
  background-color: #1b305a72;
  color: white;
}

.echarts {
  background-color: rgba(255, 255, 255, 0.919);
  display: block;
  margin: 0;
  width: 100%;
  height: 100%;
}


#loader {
  position: fixed;
  padding-top: 250px;
}

select {
  color: rgb(36, 34, 34);
  font-size: 14px;
  background: #ffffff;
  text-decoration: none;
  font-weight: 600;

  padding: 5px;
  margin-top: 20px;

  box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.25);
  border-radius: 5px;
}

select option:focus {
  background-color: black;
}

.subtitle {
  font-size: 12px;
}

input[type="text"] {
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;

  padding: 8px 20px;
  margin: 3px 0;
  width: 250px;
  height: 32px;
}

input[type="password"] {
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;

  padding: 8px 20px;
  margin: 3px 0;
  width: 250px;
  height: 32px;
}

input[type="submit"] {
  width: 84px;
  height: 31px;
  background: rgba(25, 69, 10, 0.88);
  border-radius: 17px;
  color: white;
  padding: 5px 5px;
  margin: 30px 0;
  border: none;
  cursor: pointer;
}

input[type="submit"]:hover {
  background-color: rgba(97, 140, 83, 0.66);
  color: white;
  font-weight: 600;
  transition: 0.3s;
}

.ballot {
  display: inline-grid;
  position: center;
  margin-bottom: 50px;
}

.ballot label {
  grid-column: 2;

  margin: 25px 0 0 5px;
  text-align: initial;

  font-size: 14px;
  line-height: 16px;

  color: #ffffff;
}

.ballot input {
  width: 17px;
  height: 17px;
  margin-top: 25px;

}

form {
  margin-top: 30px;
}


.subtitle p {
  font-family: Barlow;
  font-size: 12px;
  line-height: 20px;
  color: rgba(255, 251, 251, 0.65);

  padding: 0;
  margin: 0;
}

.subtitle p:first-child {
  padding: 0;
  padding-bottom: 5px;

  margin: 0;

  font-family: Barlow;
  font-weight: 600;
  font-size: 18px;
  line-height: 24px;
  color: #fffbfb;
}

.btn-confirm {
  width: 84px;
  height: 31px;
  background: rgba(25, 69, 10, 0.88);
  border-radius: 17px;
  color: white;
  padding: 5px 5px;
  margin: 30px 0;
  border: none;
  cursor: pointer;
}

.btn-confirm:hover {
  background-color: rgba(97, 140, 83, 0.66);
  color: white;
  font-weight: 600;
  transition: 0.3s;
}

.main-posts p {
  font-family: Barlow;
  font-size: 12px;
  line-height: 18px;
  color: rgba(255, 251, 251, 0.65);

  padding: 0;
  margin: 0;
}

.main-posts p:first-child {
  padding: 0;
  padding-bottom: 5px;

  margin: 0;

  font-family: Barlow;
  font-weight: 600;
  font-size: 18px;
  line-height: 24px;
  color: #fffbfb;
}


span b {
  color: white;
  font-size: 14px;
  font-weight: 600;
}


header {
  font-family: Barlow;
  font-size: 18px;
  font-weight: 600;
  line-height: 18px;
  color:rgba(223, 205, 205, 0.693);

  padding: 0;
  margin-top: 10px;
}

#query-all p {
  color: white;
  
}

.btn-voter {
  display: flex;
  justify-content: flex-end;

  padding-right: 10px;
  width: 250px;
}

#btn-reset {
  padding: 5px 5px;
  margin: 30px 0;
  margin-right: 15px;

  color: black;

  height: 40px;
  width: 75px;
  background-color: #E67401;

  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  border: none;
  border-radius: 2px;

  cursor: pointer;
}

#btn-reset:hover {
  background: #a25202;
  color: white;
  font-weight: 600;
}

#input-submit {
  color: black;
  background-color: green;

  height: 40px;
  width: 75px;

  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  border-radius: 2px;
}

#input-submit:hover {
  background-color: rgba(97, 140, 83, 0.66);
  color: white;
  font-weight: 600;
  transition: 0.3s;
}

@media (min-width: 600px) {
  .routerlink {
    font-size: 16px;
    padding: 10px;
  }

  #main-menu {
    padding: 10px;
  }

  .first-menu {
    padding-bottom: 20px;
  }
}

@media (min-width: 700px) {
  /* body {
    margin: 50px 100px;
  } */
}
</style>
