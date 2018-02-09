---
layout: post
title:      "Building an unbeatable Tic Tac Toe AI"
date:       2018-02-09 07:51:40 +0000
permalink:  building_an_unbeatable_tic_tac_toe_ai
---


The first major challenge that I faced with the coding boot camp was building an unbeatable Tic-Tac-Toe AI. I have managed to create an algorithm to solve this problem, with only one move being hard coded.

The overarching logic of my program is this the move method, found below.

```
    def move(board)
      @board = board
      if o_cells.empty? && empty_cells.include?(5)
        return "5"
      end
      # Only include empty cells on the array feeding in to the freq table
      valid_selection = weighted_selection.flatten.select do |ws|
        empty_cells.include?(ws)
      end
      freq_table = valid_selection.flatten.inject(Hash.new(0)) do |k, v|
        k[v] += 1
        k
      end
      choice = valid_selection.flatten.max_by { |v| freq_table[v] }
      choice.to_s
    end
```

As the move method is the entry point, it starts by storing the board in the class. I would have preferred to have this in the initialise method, but the rspec's provided also requested for this method to take on the board. It may have even overcomplicated the set up of the computer player, but it would have saved reassignment every move.

The if statement short circuits the method to return the centre cell if "O" has not made a move. I can not see an algorithmic way to make sure that the central cell is chosen first on a second step without needlessly overcomplicating the program. As this is a simple enough rule, and it doesn't turn in to many if.. else statements, I am happy with this solution.

The next part of this method takes the weighted selection, flattens it, and selects all of the empty cells. The weighted selection is discussed further on in detail. I build a frequency table from the weighted range of winning cells and return the highest frequency response. The frequency table makes sure that the free cell with the most combinations for wins is selected. The answer is then returned as a string to satisfy the test case for this method, and also to provide consistency with a human users response.

```
    def empty_cells
      result = []
      @board.cells.each_with_index { |c, i| result << i + 1 if c == " " }
      result
    end

    def x_cells
      result = []
      @board.cells.each_with_index { |c, i| result << i + 1 if c == "X" }
      result
    end

    def o_cells
      result = []
      @board.cells.each_with_index { |c, i| result << i + 1 if c == "O" }
      result
    end
```
        
The cells methods return an array of cell numbers occupied by their respective tokens, or lack thereof. It had occurred to me, and I had forgotten until now, that I could refactor the above into something such as the below:

```
  def cells(token)
      @board.cells.select { |c| c == "O" }.each_with_index.map { |c, i| c + 1 }
    end
```

It could still be worth splitting the above into two lines to break up the logic.

The opponent_weightings method sorts through the wins and takes -3 for each cell occupied by "X". The current players token would need checking if an option were available for the Human to play as "O".

```
    def opponent_weightings
      result = Array.new(8, 0)
      WIN_COMBINATIONS.each_with_index do |win, win_no|
        remaining = win - (x_cells + o_cells)
        if remaining.length > 0
          win.each { |w| result[win_no] -= 3 if x_cells.include?(w) }
        end
      end
      result
    end
```
    
The weighted selection sorts through the opponent weightings and only returns those with the lowest score, assuming there is a spare cell.
    
```
    def weighted_selection
      lowest = 0
      selection = []
      opponent_weightings.each_with_index do |weight, i|
        if weight < lowest
          selection = [i]
          lowest = weight
        elsif weight == lowest
          selection << i
        end
      end
      selection.map { |win| WIN_COMBINATIONS[win] }
    end
```
