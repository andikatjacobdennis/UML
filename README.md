# UML Diagrams Repository

This repository contains UML diagrams created with PlantUML to illustrate various software design processes. Each `.puml` file represents a different flow or structure, intended to help visualize and document software architecture, interaction flows, and other important design aspects.

## Repository Overview

This repo currently contains the following `.puml` files:
- **sign_in_flow.puml** - A UML sequence diagram detailing the user sign-in process for a generic web application. The diagram illustrates interactions between the user, frontend interface, and backend server components, including:
  - Form submission
  - Authentication and validation
  - Error handling and success flows

- **registration_use_case.puml** - A UML use case diagram for a generic user registration and verification flow. This diagram models the interactions involved in the user registration process, from form display to email verification. Key components include:
  - Displaying the registration form
  - Sending and verifying email for account validation
  - Error handling for invalid verification attempts

This repository will grow with additional diagrams, covering more scenarios and design structures over time.

## Getting Started

To view and modify these `.puml` files, you can use PlantUML with any text editor, or integrate it with an IDE for easier visualization.

### Prerequisites

1. **Java**: Ensure you have Java installed, as PlantUML requires it to run.
2. **PlantUML**: You can install PlantUML on your system or use an IDE with PlantUML support, such as Visual Studio Code with the PlantUML extension.

### Cloning the Repository

To get a local copy of this repository, run:

```bash
git clone https://github.com/yourusername/UML.git
```

## Rendering UML Diagrams

Once you have the repository cloned, you can render the `.puml` files using PlantUML.

### Using the Command Line

1. Open a terminal in the directory containing the `.puml` files.
2. Run the following command to generate a PNG or SVG file:

```bash
plantuml sign_in_flow.puml
plantuml registration_use_case.puml
```

This will create image files in the same directory, which you can open to view the diagrams.

### Using Visual Studio Code

1. Install the [PlantUML extension for VS Code](https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml).
2. Open any `.puml` file (e.g., `sign_in_flow.puml` or `registration_use_case.puml`) in VS Code.
3. Open the PlantUML preview pane (`Alt + D` on Windows/Linux or `Cmd + Shift + P` on Mac, then type “PlantUML: Preview Current Diagram”).
4. You should see the rendered UML diagram in real-time.

## Example Diagram Overview

Below is a brief description of the included `.puml` diagrams:

- **registration_use_case.puml**: This use case diagram represents the user registration and email verification process for a generic application.
    - **Actors**: 
        - *Visitor*: Initiates registration, completes verification steps.
        - *Mail Server*: Handles email sending and processing.
    - **Use Cases**:
        - Display form to the user.
        - Send verification email to confirm the user's address.
        - Verify email address through code entry or URL visit.
    - **Features**:
        - Shows both primary and alternative verification flows, including constraints for verification completion.
        - Includes conditional handling for incorrect verification attempts.

   ![registration_use_case.png](images/registration_use_case.png) *(Example rendered output of the registration use case)*

- **sign_in_flow.puml**: This sequence diagram depicts the steps in a user sign-in process.
    - User submits the sign-in form.
    - Form validation and parameter checking on the client side.
    - Authentication request sent to backend services.
    - Response handling, including success and error messages.

   ![sign_in_flow.png](images/sign_in_flow.png) *(Example rendered output of the sign-in flow)*

## Contributing

Contributions are welcome! If you want to add or improve a diagram:
1. Fork this repository.
2. Make your changes or add new `.puml` files.
3. Submit a pull request with a description of the update.
