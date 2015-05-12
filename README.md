# Scrabble
CS3 assignment (Dalton)
import java.util.Scanner;
/*Raina Lopez
 * Dalton School
 * #3055
 */
public class Scrabble {

	/**
	 * 1. J, A, V, A
	 * 2. 1, H, 11, H, 21, H
	 * 3. 1, V, 2, V, 3, V
	 * 4. 2, H, 12, H, 22, H
	 * 5. 3, V, 4, V, 5, V
	 * 6. 7, H, 6, V, 17, H
	 */

	static String word;
	private static int wordScore = 0;
	private static int letterScore;
	static char[] board = new char[] {
		1, 2, 3, 4, 5, 6, 7, 8, 9, 10,
		11, 12, 13, 14, 15, 16, 17, 18, 19, 20,
		21, 22, 23, 24, 25, 26, 27, 28, 29, 30,
		31, 32, 33, 34, 35, 36, 37, 38, 39, 40		
	};

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		char[] extra = new char[board.length];

		int letterScore = 0;

		//the word that will be manipulated
		Scanner Universal = new Scanner(System.in);
		Universal.useDelimiter(", ");
		Universal.delimiter();
		word = Universal.nextLine().toString();
		word = word.replaceAll(", ",  "");
		int counter = 0;
		while(counter<=5)
		{
			//Direction and location input
			Scanner coor = new Scanner(System.in);
			String temp = coor.nextLine();
			String[] all = temp.split(", ");
			int[] location = new int[3];
			for (int i = 0; i<all.length; i+=2){
				location[i/2] = Integer.parseInt(all[i]);}
			String[] direction = new String[3] ;
			for (int i = 1; i< all.length; i++){
				if (i == 1){direction[0] = all[i];}
				else if (i == 3){direction[1] = all[i];}
				else if (i==5){direction[2] = all[i];}
			}

			int[] totals = new int[3];
			int total = 0;


			//placing letters in the extra char array
			//first for loop goes through the three location entries
			for (int i = 0; i<location.length; i++){
				//if the corresponding direction for the first location is horizontal
				//System.out.println(location[i] + direction[i]);
				wordScore = 0;
				if (direction[i].equalsIgnoreCase("H"))
				{
					extra = new char[extra.length];
					for(int j = 0; j < word.length(); j++)
					{
						extra[location[i]+j-1] = word.charAt(j);
					}
					score(extra);
					totals[i] = wordScore;
					//System.out.println("totals[" + i + "] " + totals[i]);
				}
				//if the corresponding direction for the first location is vertical
				else if (direction[i].equalsIgnoreCase("V"))
				{
					extra = new char[extra.length];
					for(int j = 0; j < word.length(); j++)
					{
						extra[location[i]+(j*10)-1] = word.charAt(j);
					}	
					score(extra);
					totals[i] = wordScore;
					//System.out.println("totals[ " + i + "] "+totals[i]);
				}
			}
			total = Math.max(totals[0], totals[1]);
			int temp1 = Math.max(total, totals[2]);
			total = temp1;
			System.out.println(total);
			total = 0;
		}counter++;
	}//main

	public static void score(char[] extra)
	{
		int[] boardValue = new int[extra.length];
		for (int i = 0; i<extra.length; i++){
			if (extra[i] == 'A' || extra[i] == 'E')
			{
				boardValue[i] = 1;
			}
			else if (extra[i] == 'D' || extra[i] == 'R')
			{
				boardValue[i] = 2;
			}
			else if (extra[i]== 'B' ||extra[i] == 'M')
			{
				boardValue[i] = 3;
			}
			else if (extra[i] == 'V' || extra[i] == 'Y')
			{
				boardValue[i] = 4;
			}
			else if (extra[i] == 'J' || extra[i] == 'X')
			{
				boardValue[i] = 8;
			}
		}//sets letter scores for the word

		/*for (int i = 0;i<extra.length; i++){
		System.out.println("boardValue: " + boardValue[i]);
		}*/
		//System.out.println("word score: " + wordScore);
		Letterbonus(boardValue, wordScore);

	}//Letter method


	private static void Letterbonus(int[] boardValue,int letterScore) {
		// TODO Auto-generated method stub
		int wordMult = 1;
		boolean xtwo = false;
		boolean xthree = false;
		for (int i = 0; i < boardValue.length; i++)
		{
			//System.out.println("the current value of i: " + i);

			if (board[i]==3||board[i]==9||board[i]==15||board[i]==21||board[i]==27||board[i]==33||board[i]==39)
			{
				wordScore += boardValue[i] * 2;
			}
			else if(board[i]==5||board[i]==10||board[i]==20||board[i]==25||board[i]==30||board[i]==35||board[i]==40)
			{	
				wordScore += boardValue[i] * 3;
			}
			else if(board[i]==7||board[i]==14||board[i]==28)
			{
				if(boardValue[i]>0)
				{
					wordScore+=boardValue[i];
					xtwo = true;
					//System.out.println("xtwo: "+xtwo +" "+ wordScore);
				}
				else{
					wordScore+=boardValue[i];
					//System.out.println("xtwo: "+xtwo +" "+ wordScore);
				}
			}
			else if(board[i]==8||board[i]==16||board[i]==24||board[i]==32)
			{
				if(boardValue[i]>0){
					wordScore+=boardValue[i];
					xthree = true;
					//System.out.println("xthree: "+xthree + " " + wordScore);
				}
				else{
					wordScore+= boardValue[i];
					//System.out.println("xthree: " + xthree + "  " + wordScore);
				}
			}
			else
			{
				//System.out.println("not a special location");
				wordScore += boardValue[i] * 1;
				//System.out.println("add " + boardValue[i]);			
			}

		}
		if (xtwo == true){wordScore = wordScore*2; }
		else{wordScore = wordScore*1; }
		if (xthree == true){wordScore = wordScore*3; }
		else{wordScore = wordScore*1; }
		//System.out.println("wordScore: " + wordScore);
	}

	/*	private static void doubleScore(boolean xtwo){
		if(xtwo = true) {wordScore*=2;}
	}
	private static void tripleScore(boolean xthree){
		if(xthree= true){wordScore*=3;}
	}*/

}//class
