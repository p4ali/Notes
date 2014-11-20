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
 ```ruby
 def run
   ran = @current_block.drop_by_one
   if !ran
     store_current
     next_piece
   end
   @game.update_score
   draw
 end
 ```
* TetrisButton
* TetrisLabel

