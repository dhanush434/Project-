// Just Chess App (React + Tailwind + react-chessboard + chess.js + stockfish)

import React, { useState, useEffect } from "react"; import { Chess } from "chess.js"; import { Chessboard } from "react-chessboard";

export default function JustApp() { const [game, setGame] = useState(new Chess()); const [playerColor, setPlayerColor] = useState("white"); const [fenInput, setFenInput] = useState("");

const makeAMove = (move) => { const gameCopy = new Chess(game.fen()); const result = gameCopy.move(move); if (result) { setGame(gameCopy); } return result; };

const onDrop = (sourceSquare, targetSquare) => { return makeAMove({ from: sourceSquare, to: targetSquare, promotion: "q" }); };

const handleFENSubmit = () => { try { const newGame = new Chess(fenInput); setGame(newGame); } catch (err) { alert("Invalid FEN string"); } };

const loadStockfishMove = async () => { const engine = new Worker("https://cdn.jsdelivr.net/npm/stockfish@16/stockfish.js"); engine.postMessage("uci"); engine.postMessage(position fen ${game.fen()}); engine.postMessage("go depth 15");

engine.onmessage = (event) => {
  if (event.data.startsWith("bestmove")) {
    const [, move] = event.data.split(" ");
    if (move !== "(none)") {
      makeAMove({ from: move.substring(0, 2), to: move.substring(2, 4), promotion: "q" });
    }
    engine.terminate();
  }
};

};

return ( <div className="flex flex-col items-center p-6 space-y-4 bg-gray-100 min-h-screen"> <h1 className="text-3xl font-bold">Just - Custom Chess</h1>

<div className="flex space-x-4">
    <button
      onClick={() => setPlayerColor("white")}
      className={`px-4 py-2 rounded ${playerColor === "white" ? "bg-blue-500 text-white" : "bg-gray-300"}`}
    >
      Play as White
    </button>
    <button
      onClick={() => setPlayerColor("black")}
      className={`px-4 py-2 rounded ${playerColor === "black" ? "bg-blue-500 text-white" : "bg-gray-300"}`}
    >
      Play as Black
    </button>
  </div>

  <div className="flex flex-col items-center">
    <Chessboard
      position={game.fen()}
      onPieceDrop={onDrop}
      boardOrientation={playerColor}
      boardWidth={400}
    />

    <div className="mt-4">
      <input
        type="text"
        placeholder="Enter FEN"
        value={fenInput}
        onChange={(e) => setFenInput(e.target.value)}
        className="border p-2 w-80 rounded"
      />
      <button
        onClick={handleFENSubmit}
        className="ml-2 px-4 py-2 bg-green-500 text-white rounded"
      >
        Load Position
      </button>
    </div>

    <button
      onClick={loadStockfishMove}
      className="mt-4 px-4 py-2 bg-purple-600 text-white rounded"
    >
      Let Engine Play Move
    </button>
  </div>
</div>

); }

