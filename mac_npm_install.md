

sudo chown -R 501:20 "/Users/dhanyagundala/.npm"


rm -rf node_modules package-lock.json
npm cache verify
npm install


npm config set prefix ~/.npm-global


export PATH="$HOME/.npm-global/bin:$PATH"


sudo chown -R 501:20 ~/.npm


node -v
npm -v

npm install --legacy-peer-deps

npm install --force

