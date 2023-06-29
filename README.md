<!DOCTYPE html>
<html lang="pt-br">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <title>Document</title>
   <link rel="stylesheet" href="style.css">
   <script src="index.js" defer>
      //Variáveis globais 
let virtualBoard
let turnPlayer = ''
//Adiciona o evento de iniciar o jogo ao botão 'startBtn'
const startBtn = document.getElementById('startBtn')
startBtn.addEventListener('click', startGame)
//Captura todos os buttões do tabuleiro
const boardRegions = document.querySelectorAll('.regionBoard')
//Função para resetar as variáveis globais e o tabuleiro e após isso dar início ao jogo
function startGame(){
   virtualBoard = [["", "", ""], ["", "", ""], ["", "", ""]]
   turnPlayer = 'player1'
   startBtn.innerText = 'Reiniciar'
   document.querySelector('h2').innerHTML = 'Vez de: <span id="turnPlayer"></span>'
   updateTitle()
   addPlayerName()
   boardRegions.forEach( function (position){
      position.classList.remove('victory')
      position.innerText = ""
      position.disabled = false
      position.addEventListener('click', markRegion)
   })
   
}
//Função para adicionar o nome dos jogadores as caixas laterais correspondentes
function addPlayerName(){
   document.getElementById('playerName1'). innerText = document.getElementById('player1').value
   document.getElementById('playerName2'). innerText = document.getElementById('player2').value
}
//Função para atualizar o titulo que mostra o jogador da vez
function updateTitle(){
   const playerInput = document.getElementById(turnPlayer)
   document.getElementById('turnPlayer').innerText = playerInput.value
}
//Função que verifica se houve algum vencedor e se sim, retorna a o trio de posições à qual foi vencido
function verifyWinner() {
   const winPositions = []
   if (virtualBoard[0][0] && virtualBoard[0][0] === virtualBoard[0][1] && virtualBoard[0][0] === virtualBoard[0][2])
      winPositions.push("0.0", "0.1", "0.2")
   if (virtualBoard[1][0] && virtualBoard[1][0] === virtualBoard[1][1] && virtualBoard[1][0] === virtualBoard[1][2])
      winPositions.push("1.0", "1.1", "1.2")
   if (virtualBoard[2][0] && virtualBoard[2][0] === virtualBoard[2][1] && virtualBoard[2][0] === virtualBoard[2][2])
      winPositions.push("2.0", "2.1", "2.2")
   if (virtualBoard[0][0] && virtualBoard[0][0] === virtualBoard[1][0] && virtualBoard[0][0] === virtualBoard[2][0])
      winPositions.push("0.0", "1.0", "2.0")
   if (virtualBoard[0][1] && virtualBoard[0][1] === virtualBoard[1][1] && virtualBoard[0][1] === virtualBoard[2][1])
      winPositions.push("0.1", "1.1", "2.1")
   if (virtualBoard[0][2] && virtualBoard[0][2] === virtualBoard[1][2] && virtualBoard[0][2] === virtualBoard[2][2])
      winPositions.push("0.2", "1.2", "2.2")
   if (virtualBoard[0][0] && virtualBoard[0][0] === virtualBoard[1][1] && virtualBoard[0][0] === virtualBoard[2][2])
      winPositions.push("0.0", "1.1", "2.2")
   if (virtualBoard[0][2] && virtualBoard[0][2] === virtualBoard[1][1] && virtualBoard[0][2] === virtualBoard[2][0])
      winPositions.push("0.2", "1.1", "2.0")
   return winPositions
 }
 //Função que mostra o vencedor na tela, destaca a região que foi vencida, adiciona a mensagem do ganhador, desabilita os botões restantes e atualiza o score do jogador
function victory(positions){
   positions.forEach(function(position){
      document.querySelector(`[data-position= "${position}"]`).classList.add('victory')
   })
   const winnerPlayer = document.getElementById(turnPlayer).value
   document.querySelector('h2').innerText = winnerPlayer+ ' Venceu!'
   disabledButtons()
   updateScore(turnPlayer)
}
//Função para datualizar o score dos jogadores
function updateScore(turnPlayer){
   let score = document.querySelector(`#${turnPlayer}-score`)
   score.innerText = Number(score.dataset.score)+1
   score.dataset.score = Number(score.dataset.score)+1
}
//Função para desabilitar os botões restantes
function disabledButtons(){
   boardRegions.forEach(function (button){
      button.disabled = true
   })
}

function markRegion(ev){
   //Obtém os índices da região clicada
   const button = ev.currentTarget
   const position = button.dataset.position
   const rowColumn = position.split('.')
   const row = rowColumn[0]
   const column = rowColumn[1]
   button.disabled = true
   //Verifica quem foi o jogador que marcou a região e assim marca o simbolo correspondente tanto no tabuleiro visível, quanto no tabuleiro de controle
   if(turnPlayer == 'player1'){
      button.innerText = 'X'
      virtualBoard[row][column] = 'X'
   }else{
      button.innerText = 'O'
      virtualBoard[row][column] = 'O'
   }
   //Aciona a função de verificação de ganhador
   const winPositions = verifyWinner()
   //Se 'winPositions' possui algum valor, então algum jogador ganhou. Se não, verifica se ainda há algum botão não clicado, e caso não tenha adiciona uma mensagem de empate
   if(winPositions.length != 0){
      victory(winPositions)
   }else if(virtualBoard.flat().includes('')){
      turnPlayer = turnPlayer === 'player1' ? 'player2' : 'player1'
      updateTitle()
   }else{
      document.querySelector('h2').innerText = winnerPlayer+ 'Empate!'
   }
}
   </script>
</head>
<body>
   <header class="display-flexDirection-textAlign">
      <h1>Jogo da Velha</h1>
      <div id="inputsName">

         <div>
            <label for="player1">Jogador X</label>
            <input type="text" id="player1" value="Player 1">
         </div>
         <div>
            <label for="player2">Jogador O</label>
            <input type="text" id="player2" value="Player 2">
         </div>
         <button id="startBtn">Começar</button>
      </div>
      <h2>Vez de: <span id="turnPlayer"></span></h2>
   </header>
   <main>
      <div class="playerBox display-flexDirection-textAlign">
         <p id="playerName1" class="playerName"></p>
         <p>Pontos:</p>
         <p id="player1-score" data-score="0">0</p>
      </div>
      <div id="theGame">
         <button class="regionBoard" data-position="0.0"></button>
         <button class="regionBoard" data-position="0.1"></button>
         <button class="regionBoard" data-position="0.2"></button>
         <button class="regionBoard" data-position="1.0"></button>
         <button class="regionBoard" data-position="1.1"></button>
         <button class="regionBoard" data-position="1.2"></button>
         <button class="regionBoard" data-position="2.0"></button>
         <button class="regionBoard" data-position="2.1"></button>
         <button class="regionBoard" data-position="2.2"></button>
      </div>
      <div class="playerBox display-flexDirection-textAlign">
         <p id="playerName2" class="playerName"></p>
         <p>Pontos:</p>
         <p id="player2-score" data-score="0">0</p>
      </div>
   </main>
</body>
</html>
