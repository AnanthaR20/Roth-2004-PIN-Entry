# Roth-2004-PIN-Entry
You can **demo** this Password entry method [HERE](https://ananthar20.github.io/Roth-2004-PIN-Entry/)

This is a minimal re-implementation of a PIN entry method designed by Roth et al(2004). It is intended as a way to input a 4 digit password so that
an adversarial on-looker (someone snooping over your shoulder) would find it difficult to figure out your password just from watching your inputs.

# Instructions for the Demo

Pick any 4 digits you'd like to be your hypothetical password (could be any 4 numbers between 0-9).

1. Press 'Start Game'
You will see a keypad with the digits colored randomly black or white.
You will also see two buttons labeled 'Black' and 'White' in the grid.

2. Starting with your 1st digit, indicate whether it is black or white. You will do this 4 times for your first digit.
Each time your press the 'Black' or 'White' button, the colors will be shuffled.

3. Repeat this process for each digit- indicating 'Black' or 'White' 4 times per digit. Note that you will need to
re-indicate 'Black' or 'White' for the same digit each time the keypad colors are shuffled (until you've made a color judgement 4 times for the current digit)

4. At the end, press 'Toggle PIN Visibility' to see the 4 digit PIN that you entered. Hopefully it matches the password you chose at the beginning. If it does not, you may have made a mistake in entering your PIN. Also, if you see a "#" sign in the entered PIN, this means the color selections you made do not single-out any digit according to the color configurations you saw (i.e. You may have made a mistake while entering your PIN)

# Example Play-Through

Say you want to input the PIN '2376'

Press 'Start Game'. Then indicate the color of the first digit (which is '2') four times. Then indicate the color of '3' four times. Then indicate the color of '7' four times and lastly indicate the color of '6' four times. 

Press 'Toggle PIN Visibility' at the end to see if you entered your password correctly.

# Motivation behind Trapdoor Games

For fun, ask a friend to watch you input a password (on your phone or computer). Explain the rules to them, and see
if they can guess your 'password' by watching you input your digits. Make sure they have a clear view of your screen.

This type of PIN Entry method has potential applications for phone-entered passwords, ATM Machines, or Point-of-Sale devices where you may have somebody looking over your shoulder to try to see private/sensitive passwords.

