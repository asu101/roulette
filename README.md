
# Vue3-roulette

This project is a roulette application developed with Vue 3 and the vue3-roulette component library. The purpose of the application is to allow users to "spin the wheel" and win various prizes.

## Features

- A graphical roulette wheel that can be spun.
- The items on the wheel are predefined in the code and can be changed to your liking.
- Each item has an associated weight that determines the probability of it being selected.
- Items can be enabled or disabled, allowing for flexibility in configuration.
- A popup is displayed each time the wheel stops on an item, allowing the user to "accept" or "reject" the prize.
- The number of prizes won by the user is tracked, and the number of certain prizes that can be won is limited.


## Libraries
- vue3-roulette: This is the main library used to implement the roulette. It provides a customizable roulette component that can be easily integrated into any Vue application.
- vue-confetti-explosion: This library is used to provide a confetti animation that is triggered when the user wins a prize.

## Installation & Usage

Install my-project with npm

```bash
git clone https://github.com/asu101/roulette.git
```
    
```bash
npm install
```

```bash
npm run serve
```
The application will be running at http://localhost:8080/.


## Contributing

Contributions are welcome. Please open an issue or submit a pull request if you have any enhancements you would like to implement or suggest.

