<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">

  <title>Juego de adivina tu número</title>

  <style>
    html {
      font-family: sans-serif;
    }

    body {
      width: 50%;
      max-width: 800px;
      min-width: 480px;
      margin: 0 auto;
    }

    .lastResult {
      color: white;
      padding: 3px;
    }
  </style>
</head>

<body>
  <h1>Juego Adivina tu número</h1>

  <p>Hemos seleccionado un número aleatorío entre 1 a 100. Trata de adivinar el número, en un total de 10 turnos o
    menos. No te preocupes, te diremos sí el número es más alto o más bajo </p>

  <div class="form">
    <label for="guessField">Ingresa el número a adivinar: </label>
    <input type="text" id="guessField" class="guessField">
    <input type="submit" value="Ingresar el número aleatorio" class="guessSubmit">
  </div>

  <div class="resultParas">
    <p class="guesses"></p>
    <p class="intentoss"></p>
    <!-- se agrego una etiqueta para mostrar cuantos intentos lleva la persona al intentar adivinar el numero -->
    <p class="lastResult"></p>
    <p class="lowOrHi"></p>
  </div>

</body>

<script>
  let randomNumber = Math.floor(Math.random() * 100 + 1); // se modifico la cantidad ya que el caso de uso nos exige 100 numeros aleatorios y no 10

  const ATTEMPS = 10; // son 10 intentos y solo tenia 5 intentos 
  const resetParas = document.querySelectorAll('.resultParas'); // no estaba bien definido el nombre y 
  //estaba en la funcion resetGame, se traslado donde se definieron las constantes mas estetico y rapido de encontrar
  const guesses = document.querySelector('.guesses');
  const intentoss = document.querySelector('.intentoss');
  const lastResult = document.querySelector('.lastResult');
  const lowOrHi = document.querySelector('.lowOrHi'); //para llamar una clase se debe colocar . antes del nombre de la clase, => correccion ('.lowOrHi');
 // el texto intresado del formulario y el boton se cambiaron a variables ya que cambian con cada evento que hace el usuario
  var guessField = document.querySelector('.guessField');
  var enviar = document.querySelector('.guessSubmit'); // el nombre de la variable cambio por cuestion de estetica no tiene ningun error


  let guessCount = 1;
  let resetButton;
  let contador = 1; // se agrego un contador para los intentos 

  //Para la funcion checkGuess se modifico la mayor parte del codigo 
  // ya que desde el inicio piden que no permita el texto o numero si no es un entero
  // y tampoco que sea contado como un intento por el usuario 
  // para contar los intentos la opcion daba que habia ganado y acetado al numero pero estaba mal escrito
  // y mal distribuido ya que deberia de decir que perdio
  // para ver si habia ganado de igual manera decia perdiste cuando deberia decir 'Felicitaciones! adivinaste el número!'

  function checkGuess() {

    let userGuess = guessField.value;
    if (guessCount === 1) {
      guesses.textContent = 'Número escrito anterior: ';
    }

    if (isNaN(userGuess)) {
      alert("Ups... " + userGuess + " no es un número.");
      guessCount--;
      contador--;
    } else {
      if (userGuess - Math.floor(userGuess) != 0) {
        // alert ("Es un numero entero");
        alert("Es un numero decimal ");
        guessCount--;
        contador--;
      } else {
        if (userGuess === randomNumber) {
          lastResult.textContent = 'Felicitaciones! adivinaste el número!';
          lastResult.style.backgroundColor = 'black';
          lowOrHi.textContent = '';
          setGameOver();
        } else if (guessCount === ATTEMPS) {
          lastResult.textContent = '!!!Pérdistes!!!';
          lastResult.style.backgroundColor = 'red';
          setGameOver();
        } else {
          lastResult.textContent = 'Incorrecto! ';
          lastResult.style.backgroundColor = 'green';
          if (userGuess < randomNumber) {
            lowOrHi.textContent = 'El número es mayor!';
          } else if (userGuess > randomNumber) {
            lowOrHi.textContent = 'El número es menor!';
          }
        }
      }
    }
    guesses.textContent += userGuess + ' ';
    intentoss.textContent = 'intentos:  ' + contador;
    guessCount++;
    contador++;
    guessField.value = '';
    guessField.focus();

  }
  enviar.addEventListener('click', checkGuess, false); // mal escrito en el addEventListener y se le agrego un false para que se mantenga en falso hasta que se haga click en el boton


  function setGameOver() {
    guessField.disabled = true;
    enviar.disabled = true;
    resetButton = document.createElement('button');
    resetButton.textContent = 'Comienza un nuevo juego';
    document.body.appendChild(resetButton);
    // mal escrito en el addEventListener y se le agrego un false para que se mantenga en falso hasta que se haga click en el boton
    resetButton.addEventListener('click', resetGame, false); // mal escrito
  }

  function resetGame() {
    guessCount = 1;
    contador = 1;
    // se cambio el numero aleatorio al inicio para evitar cualquier eror que ocasionra estar al final
    randomNumber = Math.floor(Math.random()) + 1;
// la sentencia que resetiaba todas las etiquetas y sus mensajes 
// dejaba como nuevo una hoja en blanco y o se miraban los numeros o si se gano o perdio 
    // for(let i = 0; i < resetParas.length; i++) {
    //   resetParas[i].textContent = " ";
    // }
    // se utilizo un nueva sentencia con el que reseteaba todo y permitia volver a jugar desde cero
    document.querySelector('.guesses').innerHTML = " ";
    document.querySelector('.intentoss').innerHTML = " ";
    document.querySelector('.lastResult').innerHTML = " ";
    document.querySelector('.lowOrHi').innerHTML = " ";
    resetButton.parentNode.removeChild(resetButton);

    guessField.disabled = false;
    enviar.disabled = false;
    lastResult.style.backgroundColor = 'white';
    guessField.value = " ";
    guessField.focus();

  }
</script>

</html>
