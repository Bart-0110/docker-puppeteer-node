# docker-puppeteer-node
Puppeteer docker file with dumb-init for node

## Example
```
FROM bart0110/puppeteer-node:latest

# Copy the app
COPY . /app/
#COPY local.conf /etc/fonts/local.conf
WORKDIR app
RUN npm i

# Add user so we don't need --no-sandbox.
RUN groupadd -r pptruser && useradd -r -g pptruser -G audio,video pptruser \
    && mkdir -p /home/pptruser/Downloads \
    && chown -R pptruser:pptruser /home/pptruser \
    && chown -R pptruser:pptruser ./node_modules

# Run everything after as non-privileged user.
USER pptruser

EXPOSE 8084
ENTRYPOINT ["dumb-init", "--"]
CMD ["npm", "run", "start"]
```
