- Create VM
- Install Node.js
- Install Git
- Install medusa
- Install PostGreSQL
- Check if PostGreSQL was successful
- Create new PostGreSQL database
    - Access postgresql console
        `psql postgres` (bash)
    - Create a new user
        `CREATE USER medusa_admin WITH PASSWORD 'medusa_admin_password';` (bash)
    - Create a new database and the newly created user as the owner
        `CREATE DATABASE medusa_db OWNER medusa_admin;`
    - Grant all privileges on medusa_db to the medusa_admin user
        `GRANT ALL PRIVILEGES ON DATABASE medusa_db TO medusa_admin;`
    - Exit postgres console
        `quit`


- Go to your local computer and set up a Medusa server using the quickstart guide instructions
    - `npm install @medusajs/medusa-cli -g`
    - `medusa new my-medusa-store --seed`
    - `cd my-medusa-store`
    - `medusa develop`
- Stop the server and set up the local repo with PostGreSQL
    - Open up `medusa.config.js` in the root of your local Medusa server directory
    - Update the `database_url` configuration
        `database_url: DATABASE_URL,`
        `database_type: "postgres",`
        `// database_database: "./medusa-db.sql",`
        `// database_type: "sqlite",`
- Change `start` command in `package.json` file in the root of your local Medusa server directory
        `start: medusa migrations run && medusa develop`
- Commit changes
- Push to Git repo host (GitHub)
- Clone repo to your Oracle server

- Connect server to PostgreSQL database
    - Open up `.env` file in the root of your Oracle Medusa server and add the URL to your PostgreSQL db
        - `DATABASE_URL=postgres://medusa_admin:medusa_admin_password@localhost:5432/medusa_db`
- Add `JWT_SECRET` and `COOKIE_SECRET` to your `.env` file
    - Generate a unique random string for each of them.
- Run `npm install`
- Run `npm run start` to start your Medusa server in the backend.

- Open up access to port 9000 in Oracle Console

- Test Deployment
    - Visit `http://<your-ip-address>:9000/store/products` to test your deployment.

## Considerations

This tutorial showcased a minimal way of deploying a Medusa server on Oracle Cloud. Here are some additional configurations you can make.

- Install Redis
- Install File Storage Plugin
- Install PM2
- Install NGINX
- Configure NGINX
- Test NGINX
- Set up Nginx with HTTP/2 Support for Ubuntu
- Configure a Domain name for your Medusa 
- Install SSL certificate for your domain
- Secure your Ubuntu Server

- Connect backend to storefront
- Connect backend to store-admin

- 150.136.43.82

## Image Links

https://res.cloudinary.com/craigsims808/image/upload/v1684746915/articles/oracle-medusa/view-vcn_ssivik.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746915/articles/oracle-medusa/view-virtual-cloud-network_xjkodl.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746915/articles/oracle-medusa/vm_details_isbads.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746915/articles/oracle-medusa/select-vcn-nav-menu_josftr.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746915/articles/oracle-medusa/select-medusa-vcn_ckuuhy.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746915/articles/oracle-medusa/vm_running_u3uk1w.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746915/articles/oracle-medusa/select-medusa-compartment_dpvs7g.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746914/articles/oracle-medusa/select-start-vcn-wizard_z2kp8l.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746915/articles/oracle-medusa/successful-ssh-connection_vhc3tg.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746914/articles/oracle-medusa/select-compartments-on-nav-menu_kdcijv.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746914/articles/oracle-medusa/select-default-security-list_hx75zv.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746914/articles/oracle-medusa/select-add-ingress-rules_xtsjij.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746914/articles/oracle-medusa/select-instances-in-nav-menu_eec1da.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746914/articles/oracle-medusa/products-check_oqkvti.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746914/articles/oracle-medusa/save-private-key_tybj36.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/networking-options_qbjrm4.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/name-and-compartment_sr5rlv.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/details-for-vcn_yofjhe.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/health-check_nnbd5i.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/list-of-compartments_tjoqyd.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/list-of-ingress-rules_wn0jsq.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/click-create-instance_rrlnh2.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/generate-key-pair_yo3p2c.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746912/articles/oracle-medusa/create-vcn-with-internet-connectivity_zquzqm.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746912/articles/oracle-medusa/create-compartment_ev0cqw.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746912/articles/oracle-medusa/click-create_sjiglv.png
https://res.cloudinary.com/craigsims808/image/upload/v1684746912/articles/oracle-medusa/change-image-and-shape_jxc5ag.png

vm running