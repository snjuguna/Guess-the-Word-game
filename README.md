# Guess-the-Word-game
A simple game where the user guesses the word of a team

<!DOCTYPE html>
<?php
    $user_name="username";
    setcookie($cookie_name, time() + (86400 * 30), "/");
?>

<html lang="en">
<head>
	<title>Guess the Word!</title>
	<link href="worldcup.css" rel="stylesheet" type="text/css">
	<style>
body {
	text-align: center;
	background-image:url(soccerpitch1.jpg);
	background-attachment: fixed;
    height: 100%; 
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
	color: #000;
}
h1.header {
	font-size: 45px;
	color: blue;
	font-weight: bold;
}
#newGame {
	font-size: yellow;
}
.incorrect {
	color:#FF0000;
}
#sp1 {
	color: #006400;
}
p {
	color: #00FFFF;
	font-size: 40px;
	font-weight: bold;
}
.tooltip {
    position: relative;
    display: inline-block;
    border-bottom: 1px dotted black;
}

.tooltip .tooltiptext {
    visibility: hidden;
    width: 500px;
    background-color: light-blue;
    color: coral;
    text-align: center;
    border-radius: 6px;
    padding: 5px 0;

    /* Position the tooltip */
    position: absolute;
    z-index: 1;
}

.tooltip:hover .tooltiptext {
    visibility: visible;
}
	</style>
</head>

<body>
<div class="pageContainer">
	<h1 class="header">Let's Play <span id="sp1">Guess the Word! </span> (World Cup Edition)</h1> 
<h2>Category: "COUNTRIES"</h2>
<br>
	
 <!--<label>Category</label>
	<select name="font-size">
		<option value="12px" selected="selected">Countries</option>
		<option value="8px">Players</option>
	</select> 
	
	<br> -->
<h2> 
	<div class="tooltip"><i>Hint</i>
		<span class="tooltiptext"> Country that made it into the quarter-finals</span>
	</div>
</h2>

<?php

        $alphabet = array('A','B','C','D','E','F','G',
                        'H','I','J','K','L','M','N',
                        'O','P','Q','R','S','T','U',
                        'V','W','X','Y','Z');

                    if (empty($_POST)) {
                            $sentence = explode("\n", file_get_contents('wordlist.txt'));
                            $correct = array_fill_keys($alphabet, '-');
                            shuffle($sentence);
							/* if ($sentence == 'Russia') {echo "Hint: host country";} */
                            $word = strtoupper($sentence[0]);
                            $incorrect = array();
                            $text = str_split($word);
                            $correctstr = serialize($correct);
                            $incorrectstr = serialize($incorrect);
                            $reveal = '';
                            
                            foreach ($text as $letters) {
                                $reveal .= $correct[$letters];
                            }
                    }
                    else {
                            $word = $_POST['word'];
                            $guess = strtoupper($_POST['guess']);
                            $correct = unserialize($_POST['correctstr']);
                            $incorrect = unserialize($_POST['incorrectstr']);
                            $text = str_split($word);

                        if (stristr($word, $guess)) {
                            $reveal = '';
                            $correct[$guess] = $guess;
                            $text = str_split($word);
                            
                            foreach ($text as $letters) {
                                $reveal .= $correct[$letters];
                            }
                        }
                        else {
                            $reveal = '';
                            $incorrect[$guess] = $guess;

                        if (count($incorrect) == null ) {
                            $reveal = $word;
                        }
                         else {
                            foreach ($text as $letters) {
                                $reveal .= $correct[$letters];
							}
                        }
						}
							$correctstr = serialize($correct);
							$incorrectstr = serialize($incorrect);
					}               
?>

<h2>Incorrect Letters Guessed: <div class="incorrect"> <?php echo implode(', ', $incorrect) ?></h2> </div>

	<h1><?php	echo $reveal ?>	</h1>
	

    <form name = "inputForm" action = "" method = "post">
		
		<h2>Your Guess: <input name = "guess" pattern="[A-Za-z]" type = "text" placeholder="Type a letter here" maxlength="1"></h2>
		<input type='hidden' name='word' value='<?php echo $word ?>'>
		<input type='hidden' name='correctstr' value='<?php echo $correctstr ?>'>
		<input type='hidden' name='incorrectstr' value='<?php echo $incorrectstr ?>'>
		<input type='submit' value='Check'/>
    </form><br>
	<br> <br><br><br><br><br><br>

	<a href='worldcup.php'><p>New Game</p></a>
</div>
</body>
</html>
