FROM node:16
RUN mkdir /lmsfe
WORKDIR /lmsfe
COPY ..
RUN npm install
RUN npx prisma generate
RUN npx prisma db push
RUN npm run build
EXPOSE 8080
CMD ["npm", "start"]
CMD ["node", "build/index.js"]

