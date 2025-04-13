# Bot Commands

The Telegram bot provides a set of intuitive commands to manage file uploads, download links, user sessions, and verification. These commands allow authorized users to interact seamlessly with the system. Below is the complete list of available commands, accessible by typing `/help` in the chat.

## Available Commands

- **/start**  
  Initiates interaction with the bot. This command welcomes the user and prepares the bot for further commands. If the user is not registered, it prompts them to begin the registration process.  
  Example: Use `/start` to begin using the bot after launching it for the first time.

- **/registration**  
  Starts the user registration process by requesting an email address. The bot sends a verification email with a unique code or link to confirm the user’s identity. Once verified, the user gains access to file upload and link management features.  
  Example: Enter `/registration` and follow the prompts to provide your email for account setup.

- **/load**  
  Allows the user to upload a file to the bot. The file is stored securely via the Telegram API, and the bot generates a unique download link. This link is saved in the user’s personal storage for later access.  
  Example: Use `/load`, then send a file to receive a shareable download link.

- **/storage**  
  Retrieves a list of all download links associated with the user’s account. This command displays the URLs of files previously uploaded via `/load`, enabling easy access to stored files.  
  Example: Type `/storage` to view all your saved file links in one place.

- **/clear**  
  Deletes all saved file links from the user’s storage. This command is useful for resetting```markdown
  resetting the user’s link history, ensuring no outdated or unwanted links remain. A confirmation prompt may be required to prevent accidental deletion.  
  Example: Run `/clear` to remove all your stored links and start fresh.

- **/cancel**  
  Cancels the current operation or command in progress
  progress. If the user is in the middle of a multi-step process (e.g., registration or file upload), this command aborts it and returns the bot to a neutral state.  
  Example: Use `/cancel` to stop an ongoing registration if you change your mind.

- **/logout**  
  Ends the user’s session with the bot, effectively logging them out. After this command, the user must re-authenticate or restart interaction with `/start` to continue using the bot.  
  Example: Enter `/logout` to securely end your session.

- **/help**  
  Displays the list of all available commands along with brief descriptions (as shown here). This command serves as a quick reference for users to understand the bot’s capabilities.  
  Example: Type `/help` to see this command list at any time.

## Usage Notes

- Commands are processed by the **Dispatcher** and **Node** microservices, which handle Telegram messages and command logic, respectively.
- Some commands (e.g., `/registration`, `/load`) involve multi-step interactions, managed by a Spring State Machine in the Node Service to ensure smooth workflows.
- All file operations (uploads and downloads) are secured, and links are accessible only to authorized users.
- For detailed technical implementation, refer to the project’s microservice documentation.