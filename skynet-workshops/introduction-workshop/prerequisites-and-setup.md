# Prerequisites & Setup

## Prerequisites

To get started you'll need to have:

* Node.js installed
* Yarn installed
* Git installed
* Clone of the Workshop Repo

### Node.js Installation

[Node.js](https://nodejs.dev/) is a runtime for Javascript that lets you run and build applications in Javascript outside of a web browser.

Visit the [Node.js Download](https://nodejs.org/en/download/) page for binary packages, or visit [https://nodejs.dev/](https://nodejs.dev/) for installation right from your console.

### Yarn installation

[Yarn](https://yarnpkg.com/) is the package manager we'll use for downloading and keeping track of the dependencies we need for our project.

To install, run `npm install -g yarn` in your terminal.

### Git Installation

Git is a tool used to manage your project's source code.

To install, follow the instruction [available here](https://gist.github.com/derhuerst/1b15ff4652a867391f03) or try [Github Desktop](https://desktop.github.com/). 

### Clone the Workshop

To clone the repo, open up your terminal, navigate to the folder you want to put the code, and run

```bash
git clone https://github.com/SkynetLabs/skynet-workshop.git
```

## Setup

### Install Dependencies

Open your terminal to the cloned repo folder and run `yarn install` to install the project's dependencies. This can take a while and will install over a thousand code libraries in the `node_modules` folder. We're building on the shoulders of giants!

### Start Development Server

Run `yarn start` to spin up a local development server that serves our application. If your browser doesn't launch, visit `localhost:3000`. Create React App will auto-reload when you save a file, but sometimes you may need to stop the server and run the command again. \(Use Ctrl+C in the terminal to stop your app.\)



