## Runner
* Create Tetris -> Event Loop

### Tetris
* TetrisRoot
 * TkRoot
  * key_bindings
* TetrisTimer
 * timer.start(board.delay, proc{board.run; call timer again}) 
 ```ruby
 def run_game
   if !@board.game_over? and @running
     @timer.stop
     @timer.start(@board.delay,(proc(@board.run; run_game})
   end
 end
 ```
* Board
 * TetrisCavas
* TetrisButton
* TetrisLabel

