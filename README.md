AQA-2014-CODE-
==============

Annotated this code thanks to my guy Tictac

I've done quite a lot so far and will try to add more as we come close to COMP1


 
'Skeleton Program code for the AQA COMP1 Summer 2014 examination
'this code should be used in conjunction with the Preliminary Material
'written by the AQA COMP1 Programmer Team
'developed in the Visual Studio 2008 (Console Mode) programming environment (VB.NET)

Module CardPredict

    Const NoOfRecentScores As Integer = 3 'Declaration of the maximum amount of recent score

    Structure TCard 'Structure used to create the card deck used in the game
        Dim Suit As Integer 'The variable used to identify the suit of a card from the deck.txt file
        Dim Rank As Integer 'The varaible used to identify the rank of a card from the deck.txt file
    End Structure

    Structure TRecentScore 'Structure used to save player score and name from a previous game
        Dim Name As String 'The variable that stores the current player name 
        Dim Score As Integer 'The variable that stores the current players score
    End Structure

    Sub Main() 'The Sub main contains the code that will be run first before anything else'
        Dim Choice As Char 'This is the declaration of the variable that will store the menu choice of the player
        Dim Deck(52) As TCard 'This is the declaration of the deck which is a parameter
        Dim RecentScores(NoOfRecentScores) As TRecentScore 'Declare the recentscores using the no. of recent scores as its parameter
        Randomize() 'This is the initalization of the random number generator,which will be used in the code to shuffle the deck
        Do 'Intialization of DO loop
            DisplayMenu() 'Initialization of DisplayMenu subroutine ,which will display menu...
            Choice = GetMenuChoice() 'The choice variable is a MOST RECENT PLACE HOLDER that uses the subroutine GetMenuChoice() to find the user's input
            Select Case Choice 'Initialization of select statement using the choice variable as its argument 
                Case "1" 'If user has inputed a "1" it will run this case statement
                    LoadDeck(Deck) 'The case of "1" will lead to the LoadDeck routine being run using the deck variable as its parameter
                    ShuffleDeck(Deck) 'The shuffledeck routine then shuffles the deck array
                    PlayGame(Deck, RecentScores) 'The play game routine runs using the shuffled deck array and recentscores as its parameters
                Case "2"    'If 2 is inputed then the Main will run this case statement
                    LoadDeck(Deck)
                    PlayGame(Deck, RecentScores)
                Case "3"  'This input will lead to the case statement that will out put the recent scores
                    DisplayRecentScores(RecentScores) 'he displayRecentScores routine is run using RecentScore array as parameter
                Case "4" 'This input will reset the RecentScores
                    ResetRecentScores(RecentScores) ' ResetRecentScores routine is run using RecentScores array as its parameter
            End Select 'Select Statement ends
        Loop Until Choice = "q" 'The user input of "q" will lead to the termination of the SubMain
    End Sub 'End of sub main        

    Function GetRank(ByVal RankNo As Integer) As String 'Function that finds rank of gard using the value of RankNo as its argument
        Dim Rank As String = "" 'Declaration of Rank variable this will be constantly changing with every new card
        Select Case RankNo 'Initialization of select case which will find the correct rank of the current card
            Case 1 : Rank = "Ace"
            Case 2 : Rank = "Two"
            Case 3 : Rank = "Three"
            Case 4 : Rank = "Four"
            Case 5 : Rank = "Five"
            Case 6 : Rank = "Six"
            Case 7 : Rank = "Seven"
            Case 8 : Rank = "Eight"
            Case 9 : Rank = "Nine"
            Case 10 : Rank = "Ten"
            Case 11 : Rank = "Jack"
            Case 12 : Rank = "Queen"
            Case 13 : Rank = "King"
        End Select
        Return Rank 'The main purpose of a Function which discerns it from a Procedure/Subroutine, it returns the value of RANK
    End Function

    Function GetSuit(ByVal SuitNo As Integer) As String ' Same thing as Get Rank Ya bish
        Dim Suit As String = ""
        Select Case SuitNo
            Case 1 : Suit = "Clubs"
            Case 2 : Suit = "Diamonds"
            Case 3 : Suit = "Hearts"
            Case 4 : Suit = "Spades"
        End Select
        Return Suit
    End Function

    Sub DisplayMenu() ' The subroutine that displays the menu (Eazy shiz dont worry bout it )
        Console.WriteLine()
        Console.WriteLine("MAIN MENU")
        Console.WriteLine()
        Console.WriteLine("1.  Play game (with shuffle)")
        Console.WriteLine("2.  Play game (without shuffle)")
        Console.WriteLine("3.  Display recent scores")
        Console.WriteLine("4.  Reset recent scores")
        Console.WriteLine()
        Console.Write("Select an option from the menu (or enter q to quit): ")
    End Sub

    Function GetMenuChoice() As Char 'This function is used to read and the return of the user's input
        Dim Choice As Char 'Declaration of choice as a CHAR therefore it can be a number or letter
        Choice = Console.ReadLine 'Choice is equal to the user's input
        Console.WriteLine()
        Return Choice 'Return choice value
    End Function

    Sub LoadDeck(ByRef Deck() As TCard) 'Subroutine used for loading the deck
        Dim Count As Integer   ' This variable will be used as a STEPPER because it increments
        FileOpen(1, "deck.txt", OpenMode.Input) 'This will open the deck.txt file
        Count = 1 'The stepper is given a value of 1
        While Not EOF(1) ' While we have not reached the end of the deck , the EOF function will return a boolean value of true when it has reached the end
            Deck(Count).Suit = CInt(LineInput(1)) 'While count is at it's current value, the current line of text is casted to an integer and assigned to Deck(Count).Suit
            Deck(Count).Rank = CInt(LineInput(1)) 'While count is at it's current value, the current line of text is casted to an integer and assigned to Deck(Count).Rank
            Count = Count + 1 'The stepper 'counter' is incrementing by 1 here
        End While
        FileClose(1) 'File is closed
    End Sub

    Sub ShuffleDeck(ByRef Deck() As TCard) 'This routine is for the shuffling of the deck
        Dim NoOfSwaps As Integer ' Declaration of the variable that will hold the value of the NoOfSwaps
        Dim Position1 As Integer ' Declaration of the integer value of Position1
        Dim Position2 As Integer ' Declaration of the integer value of Position2
        Dim SwapSpace As TCard   ' Declaration of blahblah
        Dim NoOfSwapsMadeSoFar As Integer ' Declaration of NoOfSwapsMadeSoFar which is an integer value
        NoOfSwaps = 1000 'NoOfSwaps is assigned a value of 1000, thus indicating that the deck is shuffled 1000x 
        For NoOfSwapsMadeSoFar = 1 To NoOfSwaps  ' Fixed For loop which will go from 1 to 1000
            Position1 = Int(Rnd() * 52) + 1 'Int - returns integer portion of value( don't want any decimal shiz) / Rnd() will return a random number which is then * by 52 and added by 1
            Position2 = Int(Rnd() * 52) + 1 'Int - returns integer portion of value( don't want any decimal shiz) / Rnd() will return a random number which is then * by 52 and added by 1
            SwapSpace = Deck(Position1)  'Takes a random card from the deck array and makes it equal to TCARD swapspace/ Swapspace is a temporary holder
            Deck(Position1) = Deck(Position2) 'Basically in the next 2 lines it is swapping of two cards but this will happen to every card in the deck as this loop continues for 1000x
            Deck(Position2) = SwapSpace ' MOST RECENT PLACE HOLDERS
        Next
    End Sub

    Sub DisplayCard(ByVal ThisCard As TCard) 'Sub for displaying the current card to the player
        Console.WriteLine()
        Console.WriteLine("Card is the " & GetRank(ThisCard.Rank) & " of " & GetSuit(ThisCard.Suit))
        Console.WriteLine()
    End Sub

    Sub GetCard(ByRef ThisCard As TCard, ByRef Deck() As TCard, ByVal NoOfCardsTurnedOver As Integer) 'This sub is used to get the current card and keep count on the amount cards on the cards which is 52
        Dim Count As Integer
        ThisCard = Deck(1)
        For Count = 1 To (51 - NoOfCardsTurnedOver)
            Deck(Count) = Deck(Count + 1)
        Next
        Deck(52 - NoOfCardsTurnedOver).Suit = 0
        Deck(52 - NoOfCardsTurnedOver).Rank = 0
    End Sub

    Function IsNextCardHigher(ByVal LastCard As TCard, ByVal NextCard As TCard) As Boolean 'This sub determines whether the next card is higher
        Dim Higher As Boolean ' The declaration of Higher which is a boolean variable
        Higher = False ' assinging the boolean Higher with the value of true
        If NextCard.Rank > LastCard.Rank Then ' If statement initialized using Tcard parameters 
            Higher = True ' higher made true
        End If
        Return Higher  ' Return value of higher which can be either true or false
    End Function

    Function GetPlayerName() As String ' Function which finds Playername after game
        Dim PlayerName As String
        Console.WriteLine()
        Console.Write("Please enter your name: ")
        PlayerName = Console.ReadLine

        Do While PlayerName = "" Or IsNumeric(PlayerName) ' My little edit makes it invalid to enter a number or a blank space for player name

            Console.WriteLine("Player name is invalid please enter a valid name")
            PlayerName = Console.ReadLine()

        Loop

        Console.WriteLine()
        Return PlayerName
    End Function

    Function GetChoiceFromUser() As Char ' Gets user choice during the game , whether the player thinks the next card is higher or not
        Dim Choice As Char
        Console.Write("Do you think the next card will be higher than the last card (enter y or n)? ")
        Choice = Console.ReadLine
        Return Choice
    End Function

    Sub DisplayEndOfGameMessage(ByVal Score As Integer) ' If the Gameover boolean variable becomes true in the PLAYGAME procedure then this sub is intialized 
        Console.WriteLine()                              ' uses the current score as a parameter
        Console.WriteLine("GAME OVER!")
        Console.WriteLine("Your score was " & Score)
        If Score = 51 Then
            Console.WriteLine("WOW!  You completed a perfect game.")  ' If your psychic and win the game , you will receive this message
        End If
        Console.WriteLine()
    End Sub

    Sub DisplayCorrectGuessMessage(ByVal Score As Integer) ' If you get the guess right this message will appear, score is passed as a parameter 
        Console.WriteLine()
        Console.WriteLine("Well done!  You guessed correctly.")
        Console.WriteLine("Your score is now " & Score & ".")
        Console.WriteLine()
    End Sub

    Sub ResetRecentScores(ByRef RecentScores() As TRecentScore) ' Resets the recent score, passing the recentscores array
        Dim Count As Integer
        For Count = 1 To NoOfRecentScores ' count used as a stepper variable , so that every element of array is erased
            RecentScores(Count).Name = ""
            RecentScores(Count).Score = 0
        Next
    End Sub

    Sub DisplayRecentScores(ByVal RecentScores() As TRecentScore) ' Displays the score of current recentscores
        Dim Count As Integer ' Stepper variable
        Console.WriteLine()
        Console.WriteLine("Recent scores:")
        Console.WriteLine()
        For Count = 1 To NoOfRecentScores
            Console.WriteLine(RecentScores(Count).Name & " got a score of " & RecentScores(Count).Score) ' Goes through every  element in recent scores
        Next
        Console.WriteLine()
        Console.WriteLine("Press the Enter key to return to the main menu") ' No more code in this procedure , Therefore returns to submain
        Console.WriteLine()
        Console.ReadLine()
    End Sub

    Sub UpdateRecentScores(ByRef RecentScores() As TRecentScore, ByVal Score As Integer) ' Sub for updating scores
        Dim PlayerName As String ' Name of current player
        Dim Count As Integer ' this will be used as a gather in the following algorithim
        Dim FoundSpace As Boolean ' A boolean variable which will tell us whether there is a empty space in the recent scores 
        PlayerName = GetPlayerName() ' Recalls playername
        FoundSpace = False ' foundspace is set to False before algorithim is run
        Count = 1
        While Not FoundSpace And Count <= NoOfRecentScores ' while Foundspace is not false and count is < and = to NoOfRecentscore
            If RecentScores(Count).Name = "" Then ' A space found in playername makes Foundspace TRUE
                FoundSpace = True
            Else ' Else continue looking at other elements
                Count = Count + 1
            End If
        End While
        If Not FoundSpace Then ' If no space is found
            For Count = 1 To NoOfRecentScores - 1  ' moves elements up in the array
                RecentScores(Count) = RecentScores(Count + 1)
            Next
            Count = NoOfRecentScores
        End If
        RecentScores(Count).Name = PlayerName ' Both score and playername are updated to the most recent
        RecentScores(Count).Score = Score
    End Sub

    Sub PlayGame(ByVal Deck() As TCard, ByRef RecentScores() As TRecentScore) ' both the Deck array and Recentscores array are passed for the PLAYGAME sub
        Dim NoOfCardsTurnedOver As Integer 'Declared as an integer
        Dim GameOver As Boolean ' Gameover is either True or Flase
        Dim NextCard As TCard ' Next and last card use TCard type because they ave suits and ranks
        Dim LastCard As TCard
        Dim Higher As Boolean 'The next card can either be a higher rank or not
        Dim Choice As Char 'The choice is 'Y' or 'N'
        GameOver = False ' Good practice Gameover is already assigned the value False
        GetCard(LastCard, Deck, 0) '
        DisplayCard(LastCard) ' Displays last card taken from the deck to the player
        NoOfCardsTurnedOver = 1 ' NoOfCards TO is set to 1 obvioulsy because there are 52 - NoOfCardsTurnedOver cards available
        While NoOfCardsTurnedOver < 52 And Not GameOver ' Basically the following algorithim will run 
            GetCard(NextCard, Deck, NoOfCardsTurnedOver) 'while there are less than 52 cards turnt over and gameover is false
            Do ' Gets user choice
                Choice = GetChoiceFromUser()
            Loop Until Choice = "y" Or Choice = "n"
            DisplayCard(NextCard) ' Displays next card
            NoOfCardsTurnedOver = NoOfCardsTurnedOver + 1 ' Increments the amount of cards turnt over
            Higher = IsNextCardHigher(LastCard, NextCard)  ' highers boolean value will depend on whether last card is higher than the next
            If Higher And Choice = "y" Or Not Higher And Choice = "n" Then ' IF statement stating it should execute the follwing code
                DisplayCorrectGuessMessage(NoOfCardsTurnedOver - 1)   ' If the player's choice was right
                LastCard = NextCard ' last card becomes the previously guessed card
            Else
                GameOver = True  ' IF player was wrong gameover will become true
            End If
        End While
        If GameOver Then ' Just a bunch of easy code down 'ere
            DisplayEndOfGameMessage(NoOfCardsTurnedOver - 2)
            UpdateRecentScores(RecentScores, NoOfCardsTurnedOver - 2)
        Else
            DisplayEndOfGameMessage(51)
            UpdateRecentScores(RecentScores, 51)
        End If
    End Sub ' THE END (FINALLY)
End Module
