<!DOCTYPE html>
<html lang="pt-br">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <title>Document</title>
   <link rel="stylesheet" href="style.css">
   <script src="index.js" defer></script>
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
