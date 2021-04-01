<template>
  <div class="posts">
    <div class="main-login">
      <div id="end-castballot">
        <span>FIM</span>
      </div>
    </div>
  </div>
</template>

<style>

#end-castballot {
  margin-top: 50px;
  background-color: rgba(255, 255, 255, 0.98);
  margin-right: 30px;
  height: 400px;

  display: flex;
  flex: 1;

  align-items: center;
  justify-content: center;
}

#end-castballot span {
  color: black;
  font-size: 50px;
  font-weight: 600;
}


/* Phone */
@media (min-height: 700px) {
  #end-castballot {
    height: 500px;
  }
}

/* iPad */
@media (min-width: 1000px) {
  #end-castballot {
    height: 1000px;
    margin: 70px;
  }
  #end-castballot span {
    font-size: 120px;
  }
}

@media (min-height: 1000px) {
  #end-castballot {
    height: 800px;
  }
}

/* Web */
@media (min-width: 1100px) {
  #end-castballot {
    height: 450px;
    margin: 50px;
  }
}



</style>