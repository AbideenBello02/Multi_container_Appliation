FROM node:latest

WORKDIR /app

COPY package*.json .

RUN npm install

COPY . .

EXPOSE 80 

#default_username
ENV MONGODB_USERNAME=root 

#default_password
ENV MONGODB_PASSWORD=secret 

CMD ["npm", "start"]

