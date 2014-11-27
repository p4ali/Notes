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

## Use cases
### Game start
* board empty
* a tetromino is randomly generated
* preview is next tetromino
* tetromino started dropping, the dropping frequency is controlled by a global timer.

### tetromino rotating
* User press rotate keys and the current dripping tetromino will rotate correspondingly

### tetromino drop one row
* test if the current tetromino overlaps with the top row
* if not do nothing, return
* merge the rows and tetromino and update the rows on board
* test if this reach the bound of the borad, if reached, then game end.
* eleminate fully filled rows, and update score

### tetromino fast drop
* drop one row
* if can dorp one more row, then repeat the fast drop
* in addition, update the score based on the number of eleminated rows

