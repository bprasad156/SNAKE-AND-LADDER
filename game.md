package com.example.demo36;

import javafx.application.Application;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.fxml.FXMLLoader;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.image.Image;
import javafx.scene.image.ImageView;
import javafx.scene.layout.Pane;
import javafx.scene.paint.Color;
import javafx.stage.Stage;

import java.io.File;
import java.io.IOException;

public class game extends Application {
    public  static final  int tileSize=40,width=10,height=10;
    public static  final  int buttonLine=height*tileSize+50 ,infoLine=buttonLine-30  ;
    private  static  Dice dice=new Dice();
    private  player playerOne,playerTwo;
    private  boolean gamestarted=false,playerOneturn=false, playerTwoTurn=false;
    private Pane createContent(){
        Pane root=new Pane();
        root.setPrefSize(width*tileSize,height*tileSize+100);
        for(int i=0;i<height;i++){
            for(int j=0;j<width;j++){
                Tile tile=new Tile(tileSize);
                tile.setTranslateX(j*tileSize);
                tile.setTranslateY(i*tileSize);
                root.getChildren().add(tile);
            }
        }
        String imgg= "C:\\Ladder\\Prasad\\demo36\\src\\main\\img_1.png";
        File file = new File(imgg);

       // Image img = new Image(getClass().getResourceAsStream("Image img = new Image(getClass().getResourceAsStream(\"/img.png\"));"));
        Image img = new Image(file.toURI().toString());
        ImageView board= new ImageView();
        board.setImage(img);
        board.setFitHeight(height*tileSize);
        board.setFitWidth(width*tileSize);

// buttons
        Button playerOneButton=new Button("Player One");
        Button  playerTwoButton = new Button("Player Two");
        Button startButton = new Button("Start");
        playerOneButton.setTranslateY(buttonLine);
        playerOneButton.setTranslateX(20);
        playerOneButton.setDisable(true);
        playerTwoButton.setTranslateY(buttonLine);
        playerTwoButton.setTranslateX(300);
        playerTwoButton.setDisable(true);
        startButton.setTranslateY(buttonLine);
        startButton.setTranslateX(160);
        //Info Display
        Label playerOneLabel= new Label("Your Turn! P1");
        Label playerTwoLabel=new Label("Your Turn P2");
        Label diceLabel=new Label("Start the Game");
        playerOneLabel.setTranslateY(infoLine);
        playerOneLabel.setTranslateX(20);
        playerTwoLabel.setTranslateY(infoLine);
        playerTwoLabel.setTranslateX(300);
        diceLabel.setTranslateY(infoLine);
        diceLabel.setTranslateX(160);
        playerOne= new player(tileSize, Color.BLACK,"prasad");
        playerTwo=new player(tileSize-5,Color.WHITE,"Amit");
        //player action
        playerOneButton.setOnAction(new EventHandler<ActionEvent>() {
           @Override
           public void handle(ActionEvent actionEvent) {
               if(gamestarted) {
                   if(playerOneturn) {
                       int diceValue = dice.getRolledValue();
                       diceLabel.setText("Dice Value:" + diceValue);
                       playerOne.movePlayer(diceValue);
                       if(playerOne.isWinner()){
                           diceLabel.setText("Winner is"+playerOne.getName());
                           playerOneturn=false;
                           playerOneButton.setDisable(true);
                           playerOneLabel.setText("");
                           playerTwoTurn=true;
                           playerTwoButton.setDisable(true);
                           playerTwoLabel.setText("");
                           startButton.setDisable(false);
                           startButton.setText( " Restart");
                           gamestarted=true;

                       }
                       else{
                           playerOneturn=false;
                           playerOneButton.setDisable(true);
                           playerOneLabel.setText("");
                           playerTwoTurn=true;
                           playerTwoButton.setDisable(false);
                           playerTwoLabel.setText("Your turn"+playerTwo.getName());
                       }

                   }
               }
            }
        });
        playerTwoButton.setOnAction(new EventHandler<ActionEvent>() {
            @Override
            public void handle(ActionEvent actionEvent) {
                if(gamestarted) {
                    if(playerTwoTurn) {
                        int diceValue = dice.getRolledValue();
                        diceLabel.setText("Dice Value:" + diceValue);
                        playerTwo.movePlayer(diceValue);
                        if(playerTwo.isWinner()){
                            diceLabel.setText("Winner is"+playerTwo.getName());
                            playerOneturn=false;
                            playerOneButton.setDisable(true);
                            playerOneLabel.setText("");
                            playerOneturn=true;
                            playerTwoButton.setDisable(true);
                            playerTwoLabel.setText("");
                            startButton.setDisable(false);
                            startButton.setText( " Restart");

                        }
                        else{
                            playerOneturn=true;
                            playerOneButton.setDisable(false);
                            playerOneLabel.setText("Your Turn"+playerOne.getName());
                            playerTwoTurn=false;
                            playerTwoButton.setDisable(true);
                            playerTwoLabel.setText("");
                        }

                    }
                }
            }
        });


        startButton.setOnAction(new EventHandler<ActionEvent>() {
            @Override
            public void handle(ActionEvent actionEvent) {
                gamestarted=true;
                diceLabel.setText("Game Started");
                startButton.setDisable(true);
                playerOneturn=true;
                playerOneLabel.setText("Your Turn"+ playerOne.getName());
                playerOneButton.setDisable(false);
                playerOne.startingPosition();
                playerTwoTurn=false;
                playerTwoLabel.setText("");
                playerTwoButton.setDisable(true);
                playerTwo.startingPosition();

            }
        });
        root.getChildren().addAll(board,playerOneButton,playerTwoButton,startButton,playerOneLabel,playerTwoLabel,diceLabel,playerOne.getCoin(),playerTwo.getCoin());
       return root;
    }

    @Override
    public void start(Stage stage) throws IOException {

        Scene scene = new Scene(createContent());
        stage.setTitle("Hello!");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch();
    }
}
