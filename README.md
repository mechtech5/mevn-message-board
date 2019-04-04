# Full Stack Message Board

* [ ] Create Server Folder
  * [ ] Init npm
  * [ ] Add express morgan cors body-parser
  * [ ] Create Hello World Route
* [ ] Create client folder
  * [ ] vue create client
  * [ ] add bootswatch css
  * [ ] Homepage layout
* [ ] Add DB to server
  * [ ] Add monk joi
  * [ ] Create message model
  * [ ] Create route to add message to db
    * [ ] validate message with joi
      * username - default to anonymous
      * subject
      * message
      * image URL
      * created_at   
    * [ ] insert message to db
    * [ ] respond with inserted message
* [ ] Create new message form on client
  * [ ] Submit form - fetch POST to server
  * [ ] Show any errors
  * [ ] Show new message on page
* [ ] When page loads show all messages on page
  * [ ] Server route to GET all messages
* [ ] User can reply to a message
  * [ ] Reply appears under original message
* [ ] Group messages into categories 