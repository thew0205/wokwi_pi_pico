# Getting started with Tinyml and EdgeAI on Raspberry Pi Pico 2
This project is to get you started with running TinyML and EdgeAI on Raspberry Pi Pico 2 with executorch (pytorch for edge devices).

## Requirement
- Docker
- VS Code && Wokwi for VS Code (Optional)
- Docker extension
- Wokwi extension
- Good internet connection and atleast 20GB of disk space.

## Setting up the environment
### Using Docker
From the root of the project:

```
docker build -t tinyml -f .devcontainer/Dockerfile .
docker run -it --rm -v $(pwd):/workspace tinyml

```

open folder in vscode with container, allow to build and download it might take a bit of time.

## Simulation

To simulate this project, install [Wokwi for VS Code](https://marketplace.visualstudio.com/items?itemName=wokwi.wokwi-vscode). Open the project directory in Visual Studio Code, press **F1** and select "Wokwi: Start Simulator".

## Automated Testing

This project includes a Wokwi Automation Scenario in [blink.test.yaml](blink.test.yaml). The scenario runs the simulation for 1 second, and verifies that the LED is blinking. The scenario is run automatically on every commit, using [wokwi-ci-action](https://github.com/wokwi/wokwi-ci-action). You can also run the scenario locally, using the [wokwi-cli](https://github.com/wokwi/wokwi-cli) tool:

```
wokwi-cli . --scenario blink.test.yaml --timeout 1000
```
