<p align="center" style="width:60%">

![Logo](https://user-images.githubusercontent.com/101503258/196723802-179cfe0a-36e8-4c16-aa91-877f413e5c02.png)

</p>
 

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A website to list down available paid writing opportunities for technical writers.



## 👨‍💻 Tech Stack

![Vue JS](https://img.shields.io/badge/Vue.js-35495E?style=for-the-badge&logo=vue.js&logoColor=4FC08D)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)

## 🛠️ Installation Steps

Star and Fork the Repo 🌟 and this will keep us motivated.

Clone the repository

```bash
git clone https://github.com/ashutoshkrris/TechyWrite.git
```

Change the working directory

```bash
cd TechyWrite
```

Install dependencies

```bash
npm install
```

Run the app

```bash
npm run dev
```

## Adding a new opportunity to the list

Found a paid technical writing opportunity we don't have yet? You can easily add it using the following steps:

1. Go to the opportunities list JSON file:
   https://github.com/ashutoshkrris/TechyWrite/blob/master/src/data.json

2. Add an object to it as follows:

```json
  {
    "name": "Digital Ocean",
    "type": "Publication",
    "link": "https://www.digitalocean.com/community/pages/write-for-digitalocean",
    "categories": ["Infrastructure", "Open Source"],
    "minRate": 300,
    "maxRate": 400,
    "description": "Matches payments with donations to tech-focused charities. Further opportunities for paid updates to existing tutorials."
  }
```
* If there is fixed rate, then don't include `minRate`. Just include `maxRate`.
* If there is hourly rate, then use `hourlyMaxRate` instead of `minRate` and `maxRate`.
* If there is royalty, then use `royaltyRate` instead of `minRate` and `maxRate`.

3. Once done you can now raise a Pull Request to the main branch<br>

> Note: You can also create an issue to suggest a new source. Click [here](https://github.com/ashutoshkrris/TechyWrite/issues/new?assignees=&labels=%5Bnew-source%5D&template=new_source.yml&title=%5BNew+Source%5D%3A+) to create the issue.

## License

This project follows the [MIT License](/LICENSE).

#### Made with ♥ by <a href="https://twitter.com/ashutoshkrris/">Ashutosh Krishna</a>
[![forthebadge](https://forthebadge.com/images/badges/built-with-love.svg)](https://twitter.com/ashutoshkrris/)

<a href="https://github.com/ashutoshkrris" target="_blank">
<img src=https://img.shields.io/badge/github-%2324292e.svg?&style=for-the-badge&logo=github&logoColor=white alt=github style="margin-bottom: 5px;" />
</a>
<a href="https://www.linkedin.com/in/ashutoshkrris/" target="_blank">
<img src=https://img.shields.io/badge/linkedin-%231E77B5.svg?&style=for-the-badge&logo=linkedin&logoColor=white alt=linkedin style="margin-bottom: 5px;" />
</a>
