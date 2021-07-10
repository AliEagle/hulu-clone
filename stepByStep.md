
- Instal Next.js
  - npx create-next-app
- Install TailwindCss as a DevDepend
  - npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
  - npx tailwindcss init -p
  - Add Purge params to the tailwind.confing.js - ```'./pages/**/*.{js,ts,jsx,tsx}', './components/**/*.{js,ts,jsx,tsx}'```
  - Add JIT to tailwind.config.css - ```mode:'jit'```
  - Import Tailwind to Global CSS - ```@tailwind base; @tailwind components;@tailwind utilities;```
  - Optional - *Delete Home.module.css*
- Cleare boilerplate
- Run to test
- Create components folder, import and test
- For Images, use the Next.js ```<Image />``` tag which is by default lazy loading
  - Import module to file - ```import Image from 'next/image';```
  - ```<Image />``` will require - ```<Image src='' width={} height={} />```
  - We need to tell Next.js the domains we allow for the images by addining into the next.config.js the following inside the module.exports - *example* - ```module.exports = {
	images: {
		domains: ['links.papareact.com', 'image.tmdb.org'],
	},
};```
  - Restart server to take effect
- To add any global CSS we need to go to the globals.css file and refer to what is the global css in tailwind which is **base** - *example* ```@layer base {
	body {
		@apply bg-[#06202A] text-gray-300;
	}
}``` this will tell JIT to just before it runs apply this custom class and will generate the utilities for this params

> ### *Whenever we create a custom component we can pass **props** allows us to reuse components*

> #### For icons we can use heroicons.com that work natively with TailwindCSS

- Install heroicons into the project with - ```npm install @heroicons/react``` 
- Import the icons to be used into the file *example* ```import {
  BadgeCheckIcon,
  CollectionIcon,
  HomeIcon,
  LightningBoltIcon,
  SearchIcon,
  UserIcon,
} from '@heroicons/react/outline'``` icons can be outline or solid
- Example of use - ```<HeaderItem title="HOME" Icon={HomeIcon}/>```  here we are passing the *HomeIcon* as the props value for the **Icon component**
> ### Tailwind CSS has a class called *tracking-widest* which spaces text.
> Example of use:
```
<p className="tracking-widest">{title}</p>
```
> ### To create the effect of something showing and hiding we can use the hover class in Tailwind by initialy giving an Opacity of 0 and on hover take it to 100.
> We can also group elemnts of *for example* a NavMenue and apply this effect to all, *example*:  
```<div className="group"> <Icon className="h-8 mb-1" /> <p className="opacity-0 group-hover:opacity-100 tracking-widest">{title}</p>```

> ### Animations can be made with TailwindCSS with the *group-hover* class altough, it wont work on JIT *example*: ```<Icon className="h-8 mb-1 group-hover:animate-bounce" /> ``` depending on the version of TailwindCSS we are working with, it could also require to add the following to the TailwindCSS config file ```variants:{extend:{animation: ['group-hover'],},},```

> ### Objects can be accesses with ```<div>{Object.entries(requests).map([key, {title, url}])}</div>```

> ### Scrollbar can be hidden by installing a TailwindCSS plugin with ```npm i tailwind-scrollbar-hide``` this enhances our utility classes and now we can use where we want to hide the scrollbar by adding it as a class to the element *example* ```<div className="flex px-10 sm:px-20 text-2xl whitespace-nowrap space-x-10 sm:space-x-20 overflow-x-scroll scrollbar-hide">```

> ### To add a a transparent gradient we can use ```<div className="absolute top-0 right-0 bg-gradient-to-l from-[#06202A] h-10 w-1/12" /> ``` we need to make sure the parent element is relative so the child can be absolute to it and as in this case we are only giving the element a from color and not a to it creates a transparent gradient

> ## To implement the use of routes with Next.js we can use the useRouter hook by:
> - Importing the hook with ```import { useRouter } from 'next/router';```
> - Instatiating the hook with ```onClick={() => router.push(`/?genre=${key}`)} ``` on this example

> ### To enable server-side-rendering all we need to do is create an async function on the page we need **this is a per-page based* example: 
```
export async function getServerSideProps(context){
	const genre = context.query.genre;
	const request = await fetch(
		`https://api.themoviedb.org/3${requests[genre]?.url || requests.fetchTrending.url}`
	).then(response => response.json());
	return {
		props: {
			results: request.results,
		},
	};
};
```
> ### The above does server-side-rendering on the page creating all the information needed such as props, which can now be accessed thorugh our Home component. This is because the async function above happens before the default Home function of the component gets rendered, so by the time its invoked it already has access to the response of the async function information

> TailwindCSS *truncate* className utility can shrink/wrap text to reduce it

> ## To extend TailwindCSS class utilities for bigger screens for our app, we need to go to the tailwind.config.js file and extend the theme like this: 
``` 
	theme: {
		extend: {
			screens:{
				"3xl": "2000px",
			}
		},
	},
  ```