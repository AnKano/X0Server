1. socket.on('getUniqueID', (data) => {
    // ...
}; - сервер получает запрос на генерацию уникального ID,
генерирует его при помощи uuid и отправляет запросившему клиенту.

2. socket.on('getLobbies', (data) => {
    // ...
}); - сервер получает запрос на отправку списка лобби. Генерирует его
на основе своих внутренних данных и отправляет запросившему клиенту.

3. socket.on('createLobby', ({ name, password, makerID, makerName }) => {
    // ...
}); - сервер получает запрос на создание нового лобби. Если попытка создания
успешна, сервер посылает запросившему клиенту событие "lobbyCreated" и ID
созданного лобби. Ещё сервер посылает всем клиентам, имеющим статус "В списке лобби",
событие "lobbiesUpdated" и обновлённый массив с лобби. Если же попытка провалилась,
сервер посылает запросившему клиенту событие "lobbyIsNotCreated".

4. socket.on('joinLobby', ({ lobbyID, password, clientID, clientName }) => {
    // ...
}); - сервер получает запрос на подключение к существующему лобби. Если подключение возможно,
сервер отправляет запросившему клиенту событие "successJoin" и объект, содержащий ID лобби,
его статус и матрицу, описывающую состояние игрового поля. Ещё сервер посылает всем клиентам,
имеющим статус "В списке лобби", событие "lobbiesUpdated" и обновлённый массив с лобби. Если же
попытка провалилась, сервер посылает запросившему клиенту событие "failureJoin".

5. socket.on('ready', ({ lobbyID, clientID }) => {
    // ...
}); - сервер получает запрос на установку готовности игрока в лобби начать игру. Если после
установки готовности игрока оказывается, что оба игрока в лобби готовы начать игру, сервер
посылает клиентам в комнате событие "gameStarted" и ID игрока, который делает ход первым.
Ещё сервер посылает всем клиентам, имеющим статус "В списке лобби", событие "lobbiesUpdated"
и обновлённый массив с лобби.

6. socket.on('notReady', ({ lobbyID, clientID }) => {
    // ...
}); - сервер получает запрос на отмену готовности игрока в лобби начать игру.

7. socket.on('makeMove', ({ lobbyID, clientID, point }) => {
    // ...
}); - сервер получает запрос на совершение хода. Если ход возможен, сервер посылает
клиентам в комнате событие "moveIsCorrect" и объект, хранящий координаты поля и тип
фигуры. Затем сервер проверяет статус игры, если она закончилась, он посылает клиентам
в комнате событие "gameEnded" и объект, содержащий статус игры и ID победителя. Если
игра закончилась ничьёй, то ID победителя в объекте не будет. Если же игра продолжается,
сервер посылает клиентам в комнате событие "nowMove" и ID клиента, чьей ход наступил. Если
же ход сделать нельзя, сервер посылает запросившему клиенту событие "moveIsNotCorrect".