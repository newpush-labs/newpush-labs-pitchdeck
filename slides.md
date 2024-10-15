---
theme: ./theme
background: https://images.prismic.io/newpushcom/d0f96d86-fa05-4756-9f97-773950f93cae_main_new.png?auto=compress,format
title: Welcome to newpush labs
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
overviewSnapshots: true
fonts:
  sans: Roboto
  serif: Roboto
  mono: Fira Code
---
 
# newpush labs. 

<v-click>Experience, test, and learn the trending tech stacks with ease.</v-click>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/newpush-labs/newpush-labs" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
  <a href="https://labs.newpush.com" target="_blank" alt="Newpush Labs" title="Visit Newpush Labs"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-home />
  </a>
</div>


<style>
h1 {
  background-color: #1C63B7;
  font-size: 4rem;
  font-style: italic;
  font-weight: 900;
  font-family: 'Roboto', sans-serif;
  color: ##1C63B7;
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: statement
---
# Tired of Spending Hours Configuring Your IT  Lab?

## We’ve been there too.  

Managing containers, setting up secure networks, and getting everything to work together can be frustrating.

---

# Setting up an IT lab is overwhelming.

- **Time-Consuming**: Manually configuring every component  
- **Complexity**: Keeping services secure and reliable  
- **Lack of Guidance**: Hard to know where to start or what to do next  

---
layout: fact
---
    
# There’s a Better Way!

---
 
# What if you could have an IT lab that's:  
- **Ready Out of the Box**: Secure and reliable from day one  
- **Easy to Expand**: Add new services effortlessly  
- **Built for Learning**: Transparency to help you grow your skills  

<div v-click class="text-xl text-center">

With **newpush labs**, now you can!

</div>

---
 
 
# Imagine This...
<v-clicks>

- **Spinning up complex stacks** in minutes, not hours  
- Seeing **real-time metrics** on a sleek web dashboard  
- Accessing your services securely from **anywhere**  
- **Managing it all** with a few simple clicks  

</v-clicks>
---

# Meet the newpush labs

We’ve taken **decades of home lab experience** and created a solution that:  
- **Saves time**  
- **Reduces complexity**  
- **Empowers you to learn**  

Whether you're a student, a professional, or simply curious, **newpush labs** will work well for you.

---

# With newpush labs you get:

- **Predefined Lab Stacks**: Experiment with specific environments  
- **Dynamic Web UI**: Everything you need at your fingertips  
- **Integrated Monitoring**: Real-time metrics and alerts  
- **Infrastructure as Code**: Easily recreate and scale your setups  

---

# Predefined Lab Stacks

### **What are Lab Stacks?**  
- Pre-configured environments tailored for specific purposes  
- Examples include: Workflow Lab, AI Lab, Web Development Lab  

### **What You Can Do**:  
- Test and evaluate new technologies  
- Create complex automations  
- Run CI/CD pipelines, productivity tools, and more

<div class="text-center">

  **Lab Stacks help you get started faster and go further.**
</div>

---

# Take the Next Step

- **Get Started Now**  
- **Visit our GitHub Repo**  
- **Deploy your first Lab Stack today!**

<div class="text-center">

  We believe in a future where anyone can build their own IT lab—secure, flexible, and fun.  
  <br>
  Join us and make it happen!
</div>


---
level: 2
---

# Extendable pre-defined environments

Under the hood

````md magic-move {lines: true}

```yaml {*|2|*}
// A regular docker container
services:
  jupyter:
    image: quay.io/jupyter/base-notebook:latest
 
```

```yaml {*|5-6|*}
// Just add labels to make it access via the web
services:
  jupyter:
    image: quay.io/jupyter/base-notebook:latest
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.jupyter.rule=Host(`jupyter.${DOMAIN}`)"
      - "traefik.http.routers.jupyter.entrypoints=https"
      - "traefik.http.routers.jupyter.service=jupyter"
      - "traefik.http.routers.jupyter.tls=true"
      - "traefik.http.routers.jupyter.tls.certresolver=default"
```
 
```yaml
// Secure it with forward proxy
services:
  jupyter:
    image: quay.io/jupyter/base-notebook:latest
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.jupyter.rule=Host(`jupyter.${DOMAIN}`)"
      - "traefik.http.routers.jupyter.entrypoints=https"
      - "traefik.http.routers.jupyter.service=jupyter"
      - "traefik.http.routers.jupyter.tls=true"
      - "traefik.http.routers.jupyter.tls.certresolver=default"
      - "traefik.http.routers.jupyter.middlewares=traefik-forward-auth"
```
 

```yaml
// Make sure it's shown on the dashboard
services:
  jupyter:
    image: quay.io/jupyter/base-notebook:latest
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.jupyter.rule=Host(`jupyter.${DOMAIN}`)"
      - "traefik.http.routers.jupyter.entrypoints=https"
      - "traefik.http.routers.jupyter.service=jupyter"
      - "traefik.http.routers.jupyter.tls=true"
      - "traefik.http.routers.jupyter.tls.certresolver=default"
      - "traefik.http.routers.jupyter.middlewares=traefik-forward-auth"
      - "mafl.enable=true"
      - "mafl.title=JupyterLab"
      - "mafl.description=The latest web-based interactive development environment for notebooks, code, and data."
      - "mafl.tag=development"
      - "mafl.group=Development"
      - "mafl.link=https://jupyter.${DOMAIN}"
      - "mafl.icon.name=simple-icons:jupyter"
```
 


```yaml {*|6|*}
// And let watchtower to auto update the container 
services:
  jupyter:
    image: quay.io/jupyter/base-notebook:latest
    labels:
      - "com.centurylinklabs.watchtower.enable"
      - "traefik.enable=true"
      - "traefik.http.routers.jupyter.rule=Host(`jupyter.${DOMAIN}`)"
      - "traefik.http.routers.jupyter.entrypoints=https"
      - "traefik.http.routers.jupyter.service=jupyter"
      - "traefik.http.routers.jupyter.tls=true"
      - "traefik.http.routers.jupyter.tls.certresolver=default"
      - "traefik.http.routers.jupyter.middlewares=traefik-forward-auth"
      - "mafl.enable=true"
      - "mafl.title=JupyterLab"
      - "mafl.description=The latest web-based interactive development environment for notebooks, code, and data."
      - "mafl.tag=development"
      - "mafl.group=Development"
      - "mafl.link=https://jupyter.${DOMAIN}"
      - "mafl.icon.name=simple-icons:jupyter"
```
 
````


---
class: px-20
---


# Learn More

[Documentation](https://labs.newpush.com) · [GitHub](https://github.com/newpush/newpush-labs)

---
class: px-20
---
# Conclusion
Many tech students lack hands-on experience, limiting their ability to build real-world skills. Our open-source solution enables them to create personal labs, offering the tools to practice, experiment, and master key concepts — transforming them into confident, skilled professionals ready to excel in the evolving tech industry. 
We understand that providing IT lab environments for students can be challenging, but by offering this solution, your university can stand out from the crowd as a leader in innovative, hands-on learning experiences.
