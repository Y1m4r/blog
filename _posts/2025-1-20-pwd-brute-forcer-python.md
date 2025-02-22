---
layout: single
title: Password Brute Forcer Using Python
excerpt: "A Python script to brute-force passwords with a focus on ethical use. This program cracks passwords of length 1 to 7, using lowercase letters and numbers."
date: 2025-1-20
classes: wide
header:
  teaser: /blog/assets/images/pwd-brute-forcer/bruteforce.png
  teaser_home_page: true
  icon: 
categories:
  - Tools
---

This Python script is designed to brute-force passwords of length 1 to 7, using lowercase letters and numbers. It is intended for educational and ethical purposes only. The program demonstrates how brute-forcing works and can be modified to suit specific needs, such as cracking longer passwords or including uppercase letters and special characters.

## Ethical Use Only!
This script is for educational purposes and ethical use only. Unauthorized access to systems or data is illegal and unethical. Always ensure you have explicit permission before using this tool.

## Summary

The script brute-forces passwords on the fly, generating combinations of lowercase letters and numbers. It does not target URLs or systems but can be adapted for such purposes. Key features include:

- Password Length: Supports passwords of length 1 to 7 (can be modified for longer lengths).
- Character Set: Uses lowercase letters and numbers (can be expanded to include uppercase and special characters).
- User Input Validation: Handles invalid inputs professionally, ensuring the program runs smoothly.
- Performance: Brute-forcing speed depends on the computer's processing power.

### Program Explanation
See the code: 
https://colab.research.google.com/drive/1laYCDXGdqgxSITTKLnvaXUxp-kCrYHaN?usp=sharing 

## Algorithm
  - The user inputs a password combination (numbers, lowercase letters, or both).
  - A for loop generates brute-force combinations from 0 or a to 999999 or zzzzzz.
  - The program checks each combination against the user-provided password.
  - If a match is found, the program displays the password, the number of attempts, and the time taken.

## Key Features
  - Input Validation: Ensures the password meets the criteria (no spaces, no special characters, and a maximum length of 6).
  - Output Control: Allows the user to choose whether to display attempts (slows down the process) or run silently.
  - Performance Metrics: Tracks the time taken and the number of attempts to crack the password.

### Code Walkthrough
## Dependencies
The script uses the following Python libraries:

  - itertools: Generates combinations of characters.
  - string: Provides lowercase letters and digits.
  - re: Handles regular expressions for input validation.
  - time: Tracks the time taken to crack the password.

## Main Function: guess_password
This function performs the brute-forcing:

  - Character Set: Combines lowercase letters and digits.
  - Loop Range: Iterates through password lengths from 1 to 6.
  - Combination Generation: Uses itertools.product to generate all possible combinations.
  - Match Check: Compares each combination to the user-provided password.
  - Output: Displays the password, attempts, and time taken if a match is found.

## User Interaction
The user is prompted to choose between two tasks (A or B).

  - Task A initiates the brute-forcing process.
  - Task B is a placeholder for additional functionality.