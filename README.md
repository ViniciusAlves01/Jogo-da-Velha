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
