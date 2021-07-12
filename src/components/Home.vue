<template>
  <div class="container">
    <div class="col-center">
      <button class="button" v-on:click="startGame()">Start</button>
      <h1>Puntaje: {{score}}</h1>
    </div>
        <div class="col-sb">
            <div @click="checkSequence(1)" class="box" :class="indexSelected.includes(1) ? 'green' : ''"></div>
            <div @click="checkSequence(2)" class="box" :class="indexSelected.includes(2) ? 'yellow' : ''"></div>
        </div>
        <div class="col-center">
            <div @click="checkSequence(3)" class="box" :class="indexSelected.includes(3) ? 'blue' : ''"></div>
        </div>
        <div class="col-sb">
            <div @click="checkSequence(4)" class="box" :class="indexSelected.includes(4) ? 'red' : ''"></div>
            <div @click="checkSequence(5)" class="box" :class="indexSelected.includes(5) ? 'purple' : ''"></div>
        </div>
  </div>
</template>

<script>
export default {
  name: 'Home',

  data() {
    return {
      currentIndex: 0,
      indexSelected: [],
      inSequence: false,
      setSequence: [],
      randomArray: [1, 2, 3, 4, 5],
      sequence: 0,
      score: 0
    }
  },

  methods: {
    startGame: function() {
      this.randomArray = [1, 2, 3, 4, 5]
      this.startSequence()
    },
    
    async startSequence () {
      this.reset()
      var sequence;
      const randomArray = []
      for (let i = 1; i <= 5; i++){
        sequence = this.randomArray.splice(Math.floor(Math.random() * this.randomArray.length), 1);
        randomArray.push(parseInt(sequence))
      }
      
      for (let i = 0; i <= 5; i++) {
        await this.randomColor({ randomArray, index: i })
      }
    },

    randomColor: function({ randomArray, index }) {
      return new Promise(resolve => setTimeout(() => {
        
        if (!randomArray[index]) {
          this.indexSelected = []
          this.inSequence = false
          return resolve()
        }
        
        //Agregar secuencia
        this.setSequence.push(randomArray[index])
        // Número seleccionado
        this.indexSelected.push(randomArray[index])
        resolve()
      }, 400))
    },

    checkSequence (number) {
      if (this.setSequence[this.currentIndex] === number && !this.inSequence) {
        this.score += 10;
        this.indexSelected.push(number)
        this.currentIndex++
        //Comparar arreglos
        Array.prototype.equals = function (getArray) {
          if (this.length != getArray.length) return false;

          for (var i = 0; i < getArray.length; i++) {
            if (this[i] instanceof Array && getArray[i] instanceof Array) {
              if (!this[i].equals(getArray[i])) return false;
            } else if (this[i] != getArray[i]) {
              return false;
            }
          }
           alert('¡Ganador! :)')
          return true;
        };
        this.setSequence.equals(this.indexSelected)
      } else {
        if(this.inSequence){
          alert('¡Espera a que cargue el tablero¡ :(')
        } else {
          this.reset()
          alert('¡Perdiste! :()')
        }    
      }
    },
    reset: function() {
        this.score = 0
        this.currentIndex = 0
        this.inSequence = true
        this.indexSelected = []
        this.setSequence = []
    }
  }
}
</script>

<style>
html, body {
  height: 100%;
  overflow: auto;
  margin: 0;
  padding: 0;
}

.container {
  height: 100%;
  display: flex;
  background: black;
}

.button {
  background-color: red;
  border-radius: 2px;
  padding: 15px;
  color: white;
  font-size: 1rem
}

h1{
  color:white;
}

.col-sb {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  width: 100%;
}

.col-center {
  display: flex;
  justify-content: center;
  flex-direction: column;
  width: 100%;
}

.box {
  background: white;
  flex: .25;
}

/* Colors */
.green {
  background-color: green;
}

.yellow {
  background-color: yellow;
}

.blue {
  background-color: blue;
}

.red {
  background-color: red;
}

.purple {
  background-color: purple;
}

</style>
