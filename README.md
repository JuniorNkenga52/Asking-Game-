# Asking-Game-
import java.util.*;
import java.io.*;
/**The QuestionTree Class is the template for the 
*creation of the QuestionTree objects which plays a game with the computer.
*It has all the fields and methods of the objects of type
*QuestionTree.
*@author Nkengla Muluh Awa Junior
*@version 06/1/2018
*/
 public class QuestionTree {
  private QuestionNode overallRoot;
  private int totalGames;
  private int gamesWon;
  private UserInterface UI;
 /**Default constructor for the QuestionTree Class which initialises the new QuestionTree
 *@param UI the UserInterface object for the input and output
 */
  public QuestionTree(UserInterface UI){
  overallRoot=new QuestionNode("Computer");
  this.UI=UI;
  }
  /**Introduces a complete guessing game to the user asking questions 
  *until the user reaches an answer object to guess.Prints a message if the computer wins the game
  *else asks the user what the actual object was and a distiguishing question and makes some changes 
  *in the tree to reflect these changes
  */
  public void play(){
   totalGames++; //augmenting the number of games by one.
   overallRoot=play(overallRoot); // x=change(x) concept
  }
  private QuestionNode play(QuestionNode overallRoot){
   if(overallRoot.left==null && overallRoot.right==null){ //base case for the recursion
   UI.print("Would your object happen to be "+overallRoot.text+"? ");
     if(!UI.nextBoolean()){
      UI.print("I lose. What is your object? ");
      String response1=UI.nextLine();
      UI.print("Type a yes/no question to distinguish your item from "+overallRoot.text+":");
      String distinguish=UI.nextLine();
      UI.print("And what is the answer for your object? ");
       if(UI.nextBoolean()){
       overallRoot=new QuestionNode(distinguish,new QuestionNode(response1),overallRoot);//creating a new node is the user types yes
       }else{
       overallRoot=new QuestionNode(distinguish,overallRoot,new QuestionNode(response1));// creating a new node if the user types no
       }
       return overallRoot; // returning the new Node which has been creatied after the user response
     }else{//in this case the computer guesses right
     gamesWon++; // increasing the number of games won by the computer by one
     UI.println("I win!");
     return overallRoot; // returning the answer root
     }
   }else{
   UI.print(overallRoot.text); // asking a question to the player
    if(UI.nextBoolean()){
    overallRoot.left=play(overallRoot.left); // x=change(x) concept
   }else{
    overallRoot.right=play(overallRoot.right);
   }
 }
 return overallRoot;
}
 /**Stores the current tree state to an output file represented by the given printstream
 *@param output the reference name of the outfile file to which the tree state should be stored.
 *@throw IllegalArgumentException if the Prinstream variable referencing the file is null. 
 */
 public void save(PrintStream output){
 if(output==null) throw new IllegalArgumentException(); //throwing IllegalArgumentException if the output reference variable is null
 save(output,overallRoot); // calling the private method
 }
 private void save(PrintStream output, QuestionNode overallRoot){
 if(overallRoot!=null){
    if(overallRoot.left==null && overallRoot.right==null) output.println("A:"+overallRoot.text); //if the node is an answer
    else{ // if the node is a question
   output.println("Q:"+overallRoot.text); 
   save(output,overallRoot.left);
   save(output,overallRoot.right);
  }
   }
  }
/**Replaces the current tree by eading another tree from a file. 
*Replaces current tree nodes using information from the file.
*@param input the Scanner object to be used to scan through the file containing the new tree.
*@throw IllegalArgumentException if the Scanner object is null.
*/
public void load(Scanner input){
if(input==null) throw new IllegalArgumentException(); //throwing an exception is the scanner reference variable is null
overallRoot=null; // setting the current tree to null
overallRoot=load(overallRoot,input); //x=change(x) concept
}
private QuestionNode load(QuestionNode overallRoot, Scanner input){
 if(input.hasNextLine()){
 String toPut=input.nextLine();
   if(toPut.startsWith("Q")){ // doing recursion if the Line starts with "Q"
 overallRoot=new QuestionNode(toPut.substring(2));
 overallRoot.left=load(overallRoot.left,input);
 overallRoot.right=load(overallRoot.right,input);
 }else{
 overallRoot=new QuestionNode(toPut.substring(2)); // Just creating the node if the Line starts with A
 return overallRoot;
  }
 }
  return overallRoot;
}
/**Returns the total number of games that have been played on this tree so far by the user
*@return the total number of games
*/
public int totalGames(){return totalGames;}
/**Returns the total number of correct guesses of an object made by the computer
*@return the total correct guesses made by the computer. 
*/
public int gamesWon(){return gamesWon;}

 private class QuestionNode {
  private String text;
  private QuestionNode left;
  private QuestionNode right;
 
 private QuestionNode(String text,QuestionNode left,QuestionNode right){
  this.text=text;
  this.left=left;
  this.right=right;
 }
 private QuestionNode(String input){
 this(input,null,null);
  }
 }
}
