<template>
  <div class="container"> <br>
    <h1 class="title">Family Recipes</h1>    
    <div v-for="(rec, index) in famRecipes" :key="index">          
        <b-tooltip target="c" triggers="hover">
            Press to view
        </b-tooltip>
        <div class="openable" id="c" @click="rec.show = !rec.show"><h3 class="title">{{rec.title}}</h3></div> 
        <div v-if="rec.show">            
            <b-carousel
            id="carousel-1"
            v-model="slide"
            :interval="60000"
            controls
            indicators
            background="#ababab"
            img-width="800"
            img-height="600"
            style="text-shadow: 1px 1px 2px #333;"
            @sliding-start="onSlideStart"
            @sliding-end="onSlideEnd"
            >
            <!-- Text slides with image -->
            <b-carousel-slide v-for="(slide,index) in rec.slides" :key="index" class="carousel"
                :text="slide.text"
                :caption="slide.caption"
                :img-src="slide.imageUri"
            ></b-carousel-slide>                    
            </b-carousel>
        </div>
    </div>    
    <br><br><br><br><br>
  </div>
</template>

<script>
export default {
  components: {
  },
  data(){
    return {
        famRecipes:[
            {
                title: "Florit's Famous Spaghetti Bolognese >",
                slides: [{caption:"Usually prepared for", text:"For friday's dinners during exam's.. It's the ultimate comforter!", 
                        imageUri: "https://cdn.apartmenttherapy.info/image/fetch/f_auto,q_auto:eco,c_fill,g_auto,w_800,h_400/https%3A%2F%2Fstorage.googleapis.com%2Fgen-atmedia%2F3%2F2018%2F12%2Fbac7bccadf687899416e297cedfcd7afce9e04ca.jpeg"},
                    {caption:"Ingredients", text:"ground beef 0.7 kilo | 1.5 white onion | 1 can tomato sauce | 1 can tomato sauce in flavor of bolognse | 0.5 tp salt | 0.5 tp black ppeper | 0.5 tp cumin| 0.5 tp garlic pouder | 750 gr fresh pasta", 
                        imageUri:"https://cdn.apartmenttherapy.info/image/upload/f_auto,q_auto:eco,c_fill,g_auto,w_800,h_400/k%2FPhoto%2FRecipes%2F2021-12-spaghetti%2F211208_ApartmentTherapy_Spaghetti_0172"},
                    {caption:"Instructions", text:`Cut the onions into small cubes and then fry them on medium heat until they become golden and almost clear. When you finished move the to a plate and put aside.
                                Start frying the meat (recommended in the same pan where  the onion was) , while frying it is recommended slice the meat with the spatula in order for not having lumps later, add the salt and balck pepper while frying the meat.
                                after around 25 minutes of frying (the meat should look ready and there is no water in the pan), add in the canes of the tomato and the tomato sauce in flavor of bolognse, after adding them fill the tomato sauce can with water: around 2/3 and add it too.
                                Now put in the cumin and the garlic pouder, and add the fried onion, mix everything together and let in cook for around 7-10 minutes (depending on the pasta time).
                                While you are waiting start coocking the pasta according to manufacturer's instructions that are written on the box.
                                when the pasta is ready add it to the sauce mix it all together and serve`, 
                        imageUri:"https://cdn.apartmenttherapy.info/image/fetch/f_auto,q_auto:eco,c_fill,g_auto,w_800,h_400/https%3A%2F%2Fstorage.googleapis.com%2Fgen-atmedia%2F3%2F2018%2F12%2F20f823af9d31a7ab5f55079227cab188959e36e6.jpeg"}],                
                        show:false
            },
            {
                title: "Ilan's Incredible Hummus >",
                slides: [{caption:"Usually prepared for", text:"Every time you have Duda Le'Hummus!!", 
                        imageUri: "https://cdn.apartmenttherapy.info/image/fetch/f_auto,q_auto:eco,c_fill,g_auto,w_800,h_400/https%3A%2F%2Fstorage.googleapis.com%2Fgen-atmedia%2F3%2F2013%2F07%2F28af3f00835c1564f1806eddff49a90a0f22dc79.jpeg"},
                    {caption:"Ingredients", text:"0.5 kilo chick peas | 1 white onion | 4 garlic clove | 1 lemon | 150 gr tahini | 0.5 tp salt | tp baking soda | 0.25 tp cumin", 
                        imageUri:"https://www.mamavation.com/wp-content/uploads/2019/06/hummus-6-800x400.jpg.webp"},
                    {caption:"Instructions",text:`Spread the chick peans on the table and see if there are any that shouldnt be there, when youre done move the good ones into a pot and fill it with water for around 5 cm higher than all the chick peans there. Let it soak for around 12 hours ( recommended overnight). 
                                wash the chick peans and fill the pot again with new water again 5 cm higher than all the chick peans.
                                Start cooking on medium/high heat, when the water sarts boiling add the baking soda, and now we wait for around 2 hours. Dont forget to mix it up every once a while, and if there is no water do be afraid to add a bit.
                                The chick peas will be ready when you can squeeze it easily  between 2 fingers. 
                                When they are ready, let them cool down for around 30 minutes, after move the chick peans to the blender ( save a cup of the cooking water for later), blend for around 1-1.5 minutes, after that add the salt,pepper, tahini, lemon juice from the lemon, cumin and blend again for around 1 minute. You can now server it and wait for the appluse`, 
                        imageUri:"data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEAAoHCBYVFRgWFRYZGRgaGRoeHRwcGRgeHB4ZGhoaHBweHB4eIS4lHB4rIRocJjgmKy8xNTU1GiQ7QDs0Py40NTEBDAwMEA8QHhISHzQrJSs0NDQ0NjQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NP/AABEIAJ8BPgMBIgACEQEDEQH/xAAcAAACAwEBAQEAAAAAAAAAAAAEBQIDBgcBAAj/xAA9EAACAAQFAgQDBQcDBAMAAAABAgADESEEBRIxQVFhBiJxgRMykVKhscHRBxQjQmLh8DNykhUkQ/FEVIL/xAAZAQADAQEBAAAAAAAAAAAAAAACAwQBAAX/xAAoEQACAgICAQQCAgMBAAAAAAAAAQIRAyESMUEEE1FhIjJCkXGB8RT/2gAMAwEAAhEDEQA/AM+mEoKsbxS0mvyw7lZcWuxiRy77JiHn8npcBIMKwEBTVatDGgbBt/M0Lp2DJb5o1TRkoaFRNOYqmvDxMpY3VC3oIHm5O5vpp2MMjJCpRYlLngx6k9hyYObLnBpSKzlzHtB2gOMihsQesQWeeTDI5aoWpN4CaSo5jNHPkiaYisEJMBi7K/D83EH+EjMPtbKPVjaNVgfA6JQ4idU/Yl/gWP5Rjigllrsx7AQThcknTvklO3fSQPqY6NhcJhpX+lJWv2m8zfUwVMxxCks1ABAaW2DLN9HN5PhSYZnw3eXLb+tq+1rV94PxuVJLbSgWYT/MnykinB2uOIYZpikmMrny336kHfvArz1KkI4IuRwQx39o87J6iUtIjyZG7B0wkmXR2FWetWtpVdjQde8aXJcky+Yv+nLZx8wDlqdK+sZnC5c0yil/JQuRT7N6E9IGEphMrKUhXJGoVAO+x5p2g8OXi227BjOSOjp4fwg2w0v/AIxYMnww/wDjy/8AgIWysccPh1JImEECzVYA71PNIdiaNOqtqVr23j0IZIyWh3NlH/RsMf8AwS/+AiD+H8Id8NL/AOIgnD4lHFUYMK0qCDeLg0M0zVJ/Ipfwjgm/8Cj0LD8DAU39n+Db5da+jk/jGoBiaxlL4N5yXTMFif2ZIfknsOzKD+FISYz9nGKW6FHHY6T9D+sdbESWO4RDWeSOBY7IMTK/1JDqOumo+oqIXiWa0j9MotRe/rCzMPDGFn/PJWv2lFG+ojHj+Ao5l/JH51KUNIkFjq2cfssBq2Gm0/pe4+sYPOPDWJwxPxZTAfaF1+ojGpLsNSi+hh4FyVcTOqwqqUt1MdkSQktPKoFBHM/2WzwHdeag/dHTcaKoaRI3t/QE/wBhW08sakwVJeAVgiW9IiU92wmgiXimR6NdYKxuHV11DpC+aQY9w+IoClYshkrT6YuURQ8yjWg7Dz6wvxskq56R5JmER52TIubTQxLRo5DxZipQYCogLAPUiGWIItFvplpvwLkznEuSa+aCMHlTPqK3Aitp6iNJ4QVW1Nqv9nj1hyTZbOXFWJsHlSAn423ENcDkco1YIAOLcQ0zPJwzahUnpW0BzpExEu5UbaRc+0A012DzUlplknJxLB0UufugTEZYin5Cx7bRdhMCzm7Pp4qYbPhaD5tupjVvoBuntmYxPhtH/kKHqDCTNPDjyRWmpeo3jocnDNSrEGIY50QfxCD0UcwyKbB93izmOEyh8R5USw3Y2UepP4Q3wvhvCyLuBOcdR5AfT+b1MOp2K1DSoCJwqig/v6wA6H2hqVC55HIumYtiAB5VGyqKAD0EVVisRNRGOQuixTC3OsStAhv17QyVIQ51g5jAeVak8Vv7RN6iTcaX+wJ3x0KZtWJ1WVgNNNjSxEF4XCyAv8QlRTdbEHqa78QwweAQhfjOCeFFgPXvCPPcsLBmRgKOaJUk7V9gPziWK2k3Qhwa3Qzw0oBXSW5IYjTWgqKeap4EJMxSaGYOdCpRQFqbUtpIsa1rBeAw2qWdeosV8pUqRtsa3Bj3FORIk6ubEsbgUp9KR0ajJ+W2YwFMYEVNCvqYmtT5WBO4FN4+x2Y4t6JqdQ50BNxSgAsOYnlQV0di5CoD5SdRa9tINlMW4sBEVlagNaefzq6itwOuwh9uMtIym0FZNnz4RPhuFJqCdy21KWtx1joeGnh1DKagiscclH4hKggUFSWsp08A8mpjdeE0QeZcQSAAPh6jpB9G3v0h8Mji6f8Awbjvo2SmLQYpRTTaJqwrSt+kUqaG0XKYmIgsWUgrOoMltaLlMBI0XpMg0zKChHzywwowBB4IrEEeLVMbZhm5ng6Qk34+HHw35A+RvUcHvBwUjysKGHMRZAdxCp4YydrsLkzK4mRoPaKRGhxWGoOq/eIXPgR/KY8vP6SSdxGxmvIDW0UTKggiGH7sY9/czCPayNVQXJCyaC5rFSyb0htLwmn5jEZ2KRKm1YyWG1c3Ryl4Rbl8jSKmFHiLPVl0Grn9YEx+f8AxyzxPnRmzKKTRe/MPw3P8MfXlm8Gts1QKsK6qRrPCGfylAkvRWHyt9q/4xi8hyWfiXASqpy52p26mOoZN4dkYdbKGflmuYvfGIUpuSpjxdq1rFc1RuRePUem0XUBFLQGpIS7QIZNLgxTNw+o0YkwW7U2vTiBsfihKTWd+B3joxT0Fya2D5hj1kin81LKeO5jNTp7OSWNYGxOIZ2LMakxWHMM6Aey9pkQZyYgU6xYKDYQMpBKJ6qVgzDYct8oJ9IngMAXu50KDzavpBs7HIlZclQSB5m4HqYTOaQcYNlc74MgK0w6mt5RCvOfELgEIlK8EUNICxM8I2ofxJ3W+gDoo/OAMTiZr1byljahHHQdIknl8NjvYdWit8QlaswqR5jTnpCebiPMaFTQ0WnX09OIuxCqq3lsGNRvbr62hHjJ4YitV0k2HJoBb6R2HGpNs82fLnUhxmGJZVVhswvp44v3tCzM2Yy01FjUBt9q7j2t9YKlYhihRQQrFag3Ntz3gUkFDetDQnpDYR4+OmYoKUmkB1VSoUitL1NRUwd+/sEAYqwppUKvPr0/WAfh6g7lQABRRWxbase4ec0u1mqNuh6gxQ42hkcfE9GJ1EA0CqPKKVv8A4fuguWKFXcagSAFBoSPXiAcEjHZfMxtVeObxosnEwA6kppPlJH4QvK3FWkbixcpU9G3xGdCVJQqL6FohYEjih60gLJ8xmzGZy1iaHy2oDsIyGZBqh1JqLm8OcmxdVABtWtPxhDyVFSZZ7Vyr4OhYZyR1gkPbr6QowGYJpPLUtDCRMpQqQSdx0h8fUJrTBeJ/AWjjrFywpmSgzX56doulynF0Y0FqGHw9RfaBlhpdjQRMOYVpjypCupFeYYJMB2MURmpdCHFrsLSZFlYDBi1GvDVIEIhJnMl0UvLFaCpUdO36Q7EfRk48o0cnTOfSPFqHc+0TmeKk4MJP2nZK2H/7qSvkJ86j+U8MO3WOZtn56R5ssOe6TK4+01Z1HHeKdVQv1jPY3Otyz/fGHnZ452FIAmYhn+YkxkfQyk7mzfcjH9UO808QM9VTbrCGtY9CRJUrtF0IQgqiLdyds/UUhkRQqKAB0jwZgt6gikIEx12APlEQlz2rtYmkRqaYzgaTD4lG+Vr9IvL0MZ6TLOoesHz8SQNIv1MdvyDSG6AG4F4x/iLHa5hUHyrYfnD/AA2IISYx/lWg9TGMcEt6w+P6in2QEFKAB3iAl0jya8Y5UalZ47kmNFleVLLHxJhBbeh2X9TAnhvA1JmOKgfL69Yn4jxZ1CWtrVJhGSdKxkVbpAfiLNA1kFwYVYcO+5N9xBMvDarneD5MoLQn8D+UeLn9W22olcUoxAVl6QWfYWFqn7oNy/LlmiullAO7Ar+MaPA4IAAkfWEfirNwh0I1+Yfj9M3FSn38ArK5S4xBMxwGECnWWNK2Xfa9PpCjA5Xg55RBKOpjSrM1R6XGwiuRNNb3hjlUttYZVul/bb6w1zUGkv6RjxRdtmgn5Nh5SBRLSgHIqfqb1jGYfwp8Wa3wqJLBqa3ArwOpjV42c7ivHPaIZViykpmIoob5uCNt/WGObTtArEuP2JJ3hXCyQdSl2ryaAE9Av6wpkeGdZLIy6ATUGgIHqTcQdnWZamb3jP4fFzndUlioAJap0igpuTen4wUJydse8DUbo0owCKUUqSopcD6wyqg8pVjX5QBUjuaCEOZTmRV0TP5QNI2qRWoBv7xq/DuMV5fxAoFyGte34Rym2DLE4K5L/AvzLDIzeaUaEAEhaW7gi/tC1cslkjQVTSTpsQTXrwYeZvNJrFWClpOXQ4q1a1qLDr2gb5dDIONW0ZjFT5yOFVdrGp3i7BYuYr6nUrRvt1tGnl4QfI6ggcmhNDzXrFGJycJ5gaqduYF43WkHHLBN/YwyTEljVrm9o0UsVvtGZwYp8tQB2/y0P8Lia2O8OwSa/GRFnabtHmLwesEG3Q9IQSMyEl/hzLOOeCOD7xq0vCHxTgAyCZSujcdV/tFNOO0T3ehnIxQPIg2W8ZHDY6qgqLQ5wONDb7xRDKpASjQ8V4sgSW8Xo0PTF0UZlg0nSnlOKq6kEHuI/L+f5Q2GnvJbdGIB6r/KfcR+qTHGP2x5YBPlzgPnUo3qKsPzjpfJsdujlQkx8Fgx5NIqKQHKxjTXZBRXiHOU5YxLVQmw2Me5JgwTrfYbQ7E/RW9ATanSJc2dp8YicmStHSsswpKVPMXzMMRsIpkYlk8o4r+MX62rfpE6lEv3ZNcVUBf5olNJFq25ihW6bx5MmeYbU5rBcrZ3GhowphXI5YfTyxm1WNHgj8SROUcEkemkU/AwgXaKm/xT+ib+TKJhtFWBwpmuF4G/pH2KakPPDmG0pqpc3+u0JYa6GONwodBLV2TYakNKU/zaMfiJLSJrJMmF9qEippGzmgBgO+wFO8JfE8kF1YLq1LSvFolzxuLYeNJyB8MVIBEN8vk6mBrbci1ITYOh2FBtGkymmw6R43psSlnVlGX8YsKxs5lSqLqbpzTmg69o5b4jlzJrOT5H3oRsen946xMQ11XNB8opeOd+KcRWcPI6NS6tSo6bEgi3WPcyWnyJsTV18mdyqY8spJIJd3A1VqKMQB+u8dNmFJUsqvAuabnvGQyzDfEdCB5kbUo28wvBuc4x1U6gVJ46xLCUZtutlCxNSqyEnPFSd/EAKMfMDx0MM8fimnlZYKiW/lWh3B5pxSkcuzLHl3aWp8xKqO5Pzf8AGNX4axjfCUKxdkagBsCKGt/UGkUODUdjowjN3HtGwxCJJWiqAaXPJtuTzGSzLF0DkKKmlTzatL+8U5z4kUNpeqsu4/uLGFuKk4ycmuVIYJuGbSpI/pViCYyMHJ9aGxg8dOT2CvM1kNvUb9tr/dGl8MT5iHQillard1PNeKRnsnyt3ZZTAq9bg0qoNTUx0zKsFKw6UUDUyjU25YgdenaBap0hnqcseFdtiDNcXQ+caf8AOu0IsBjp8yf/AAkbRWhJsNPF/aNHmLAtTf1i7EZ6kqWHKgbaiKV3p5V6frHY2Rykox6PCmkaWap2v136xOTj2c6AhPc7A814hdPWbMEudhypDtU1F9G1xeh39oZ4RDcUoR2AAAH+X7QzYqMk020VpjKEq1r/AF9Itl48arEiFs8UY16b0ryKRATGIFWFmvRRWAzOXHQUYp7Nng8aDaDcQmtGU8gxlMBMvqFfr3jTyJgItU9+PrBenzOcakJyQUXow+XHRMeWSaKaCo3gzHTmRdag/wAM3A5W34bxbjpFMTU/Ka3tubwaJYA0gEi4qbxRF0zJJNDPKsUHQMIaIYyGROJUxpFbCjL/ALSTaNbLMWQlaJpKmEKYxnj/AASTQiuoIrX3/wAJjYKYyvjOdQp6gfUGCn+rOj+yMKfDOGUaihNe5oPaA53hrDMxABX3MaVpoHBJ7RdKRSe/eIny+SpSjW0YXNMF+7+RQSlKg0+sJcRjApFtxXrHTc5yz46BA+ihrWlbRkG8DvU7P0NeIGMVdyIcmNuTcejo+GrrClDqvWv4iCpsoqbi1YuxUrVzpZdj0/UR5Jx2kgTKAk0Hf9IVGnou5H03D6R5R6mE2PITUztpGw9ohnniZVqkpasK3rYRg81zhmNZr16LX8oPhb0MjfbOgeEM/RsQZANnU0J+0t6D1Gr6QVmOFMuYy8bj0O36e0cYXOpizEeV5SjKwPNVNfpHd5WITH4WXiJXzaaleQR8yHuD/l4rjF8OJPljxlyM1iVrGsytQqiM1PGx7j8Y0uXvanaJ32d4DXlkNq3FPvjKzJWJYsZoASpIWobTW1ugvDzOsVMSUzSkLsLUFKjobwtTM5rSg7S2HDKD5r1FVsL14hU6bp2bG7sBw80VpX9IcYDFBWud7f8ArrGTxZYPSpQPQgH5q7kEVseIEGtSSHaY2qtBspoQAST0t7R5XtOGXmmVyTkkq7OpJNDbMD2qNoQeJsq/eAGDBSqvQj5jWwHcWP3Qq8L4ppbv8ZlDODpFBY72AArfeE8+diZDaUKlHYmqamK0a+kHYkb1tHpe6pqiZY5RmlRVg8QUbTcEGnNQdt/eG83MWRjUFxTY3/D84qx+UFm+PLWpIBK2ND/T3/SE2Mx4VirK6XAuNz+Q/WJuDjK4nqQUJVyIzfC2tGmyRpPmIBFSam4XkGhN4WYbDJKC0DpMVjqFWHuOkajIs1Mtgrk6W7k0rsfSC8ThRPnqBK1ywPnLDexIUC5UC3rSHKblGr35CUlgk7Wn5QF4dyfDz2V5hLup1FWFqjatr71vG0xhDKQNwIWrl8qUSUOn/ae3Tn3hSk2eZhVJgCkXZkqfQUNIZF0uJJml7kuV/wBiPHZgJb69Oph5R1v6Aw1XNH0aijqlqFlI45geRkumYWdy9CaWoK9t402GxQApo36mv4xnFMyWVKtWZN8x1ljTa/b2hZg2ae7K0supNANrcehjd4lkIOpK1sL89R0hYrLJrRQDuTsa9axqSiC58kMsBhFkpRV00X5Sf8v2gPEY6gZr1fgcD8oomYh3FdRp+MCKjVqW6inHX8fxjuQPFA2LzFEOktyBtYVBF/uhbiJk9XDKS6nvQU7xdjcKHevfpyIJSZoCqzbi3W3YQjJm8JWdw8N0M8qxWokEUJpUd42eCl0Va2oLAcRl/D6K7KVuAKnevb/O0P8AGYpUTe9L80jvTRq5MzK09IRZzix8ZFBuW27CsMSlVsflO1KUhFkssO7z3BpcL0AHMOzjVZaKQ3pFSaXYuVLoUZridEyXNFPmCk9Qf7xuMDM1KDGC8QyP4Yp9uXQdDqG3rG3y+yL6RXhdoTk8DEN1jC+JJomTBXg6vrUCNNm2NVEJJoKEk9FG5ji+G8TlsW7ufJMag/pAsn3fjDZ/rQEI27NRNmqhGo7x6mLAYLrFTtFjSlcXvWF83AszVB0gC3WJGmPSTG7ZiyDzMKQMueqa6WBpvCN8AxYByWG8RxeHUUoABHI5pHVp0vV6wBicOCNLioP+WPENCI8KA73jzozd0YzmviTw1iAD+7BXB3FdLinrZvujneKwzo2maro/RwQfv3j9EPg6fJ9Ig+Sy5y/9xLRwPlDqpp3HSL8GS9NBLK49n54Q12je/s18RHDTfhOT8OYfZX2B7A7H2iPjTwVLwqGbJdtOoAo16auQ29B0NYy0oEDY0iiLTdxfQbanGmd3znKgwMyWK1uyjnuO8D5biPKOosfuhF4J8VsyCXMNWQCo508MPzjXvhEmfxJRFTuBsf0MbPHy/Jdk1uLplqG57xD4QAI26ive34RDDkg6WBEXywpN6frE9eArEmIypJyliKMG4t0OwjL42W8tyHWxNQ1LMK2uI6F8KreWy8xXisOGUqwBXpE+X06nH7KMXqOMtq0YFp5GkoAGoaGl1ruR3irLcMUNVcm5+au5/wA3pGjbJQtwpWp3r5VFa8c0tAk7L5iFiPN/UORxaIZY8sFUS5TxyeuxxgMUtBqpqp7V57XMVZhg5bqzvLGrSRqpX07Ewq0TFADKVrWtL272pWLUzMoAq3qNiRsPujV6mSXGaFPC75RYgwOEcaiysVrbyUqfUQ3wc2boYygKqtwWFqdDSm0Hz8yOkhQFbqNrdhAf76SrKraKrdqVJ/8AcZGUJStMbc5RpoHkZi7q6afOa6TbynveGuEwoRAWYFiBXY3/AEjMYOQ4Y3JpetaAiGE7GlKBAWqaW2r3rWgtBxzSi/kHLhXgb4lAzqUNAB5hS1Yslr5idQptS1iIU5bhZ8xtTLoUVILGurpSnG/0gSdlM9AdMwUrcaStRvUsKkf3h8csmraJJxektjzSpYEsLHqN69IQ+IpTtMJQllKgEC1G/MQ2lSmVNTlR0odXvEZIVmAOqrDcC0JnmzS0ohe1GtsWyXJWhBsY8XDmoAG525hpjAqUCq2o7Ei3PA/OPFnsoLUoaXAQ04FTav3wHt5Zv8nQ38UgX/praqFaVFKhhY+20Tw3h2ra5kyvS2q23aKsRmDUb4aEttrsLnmh39oJSc4l6iSWHP3bA3EOx+n4vbsXPqxomiUmhLAbnk+sIMxxjzi0tNti19r1pSCTKLqS53pya07dIKlYULsNxxFcYpLQnlRHCS1lS1Ra2HXnreLAdOwF+nX9Ypk4qW+ocoabj9e0EeWld6f57RyaYN3sSZqHd5SDdpi0FelST7UjapNCIOgFPUwjwuFGsTnqNKlUHJ1U1N2FgPrCnxZ4lEhCQRqpRQOCdgO8WYY0rEzduhJ+0bxEW/7dGu13Ndl4T3/Ad4544r0iqdiGdmdjVmNSe5iIeGNNhxpKjVZf4r+HKCMCXW1d6jj3iuZ4wb+VWrzcUMZqI1EDxRtofzfGM4iiqoPUwC+fTm3ZfpC9JZY0UEnoLmG0jw/MP+pSXUVAINfcDaNbiuzEm3o/RSivESAg74yxEzlFzEC9Il/IDn9FCySfSPMVMCj8uYpxOZqoJ46nb6xgPFHjFfNLksGa4LA7V6RzSiuMNthRi5PYs8cZwsxxLVvKDfoWvf8AD6RndB4NfSPBNRq3KnrY/WsU/vLC2pTXkCK8OJxjQxzjF0E4We6MHSxXt9Y3vh/Pyw1ISCPmQ8H9O8c3TEubauvSkTwmNeW+tTcff2PaHKMkKnKMjvOHx6TQNQv1gh8MSN6jgjiOcZNnazUsaNyK3B7dod4TPHTc6hGSin2he10agq6igYH1iE9dS1DU/M94Fwueypgo1j9ItxWHLqNDA9uT0/OEyxNL8dhwab3onOQMNNa0FPfrC4gy1YM1WPWlBQUHtT74IM1kswYDuKge/MLcbiNZIAB1WFbcb9ev0iXJr/JXig2/oofFzhLWyEEkDcV70psOsKp0wolSLs+w521EHkCsO8mlKACwppqAAaLvFOc5OZhLyjQgWFtI77xNODkh2TI4Jxgt/ImwGhy4LvUW0gDVTq1OK8Q1bLpa1Ys3H2b1qAKRn8tyaZKnA6hU2qNxXoY0EzCFZmhWoTQksCbDjeMWCNdHRyypKT2UYnK1UhdbFjWwHH1g/LcFLWjFtVBsxFAb3ob8RKXlwZtTMKKNJoCDXmvrF6rLYhAKjY17HiDjhindGSzNqrBc3xNR5SAFFDQ2ptAQxLTF001Ghr6Dm0aTGYVFQ6VUmnIBrCzATgo0ooWvRQD9YY4O9i/cXHQpweXvp0C17An39YLxOWTFC/Cfzi3m+Sla257Q9w8oqam46R9iRRq/y9PSC4aAlk5C7CsyLSbVzQcC3c2gaedZYNtTg02gvE4s78E0pCpsfL1lC3mBAIKkb7X2MdS+QXkokkoVHPl6C/5xNZaqBexvQnrBiywL9oFlyg7aqGoNj2p0gktGOV9k/iVbfUKWCjb3EAZjmLoh0SzXap/l7kQ+wqMASRpB2rb6CKsRNlKakaj3sPpzBxwyYE5ow+VYSdOcuiWFtR8qjvqPPpGmwGFXDqWdzMc7lvkXmwO57mPMVm1bDb6AegEIMzzIKpd2ooh+PBGK+RKckqD83zwIrMzUA3Y/lHKM6zNsQ5Zq6R8o7dT3MWZ3nDYhuiDZfzPf8IVw+g4qiQMfViIXpGiwHhDETKFyssH7VSwH+0frGOkbyYiDwywGAV2GttArcfzH04Ea3BeHZMoEAFn21tSveg2H4xJMgTWCxsDX1hMprwOilWxnl+UykUfBAUcnSCzW5JvBGIZCBqp7kC8UYvGCWulDQ9fawhdg8MzDVMOotQi2wPEJdsKKL2/asv8A9d6/71/GkLMx/ajOcESpSoT/ADMxanoAAIxU2XU1A+kUmSYoWKHlE9oLx2f4iaT8SaTX29rbCA5OLZTaJ/upiSyIZ+KWkarfkKk5pTcGvYiDFzGWd1b7j9wivD5BiWuuHmsOolOR9dNIImZHMQVmYaYgHLI4H1pSOUkY4sks+Udnp6iLNSEVDqYEXCLTYj3j1sMvJNPaDti6TCZU1kbUhow5EabLc/V/K/lfvs3pGRfCIVtUMN6gUIvtTaBDLavzD7/1jmrM6Omlovw+ZTE+VjTpHPMBnk2VZvOvN/MPTrGkwebpMFVYV6cj2jKZqdm1w3i1hZ1rDCVn2Hf5gAfoYwhcHmKWgWk+0ErXR0yV8ErpRwK9Tqi9cOAmlXBtTpHKxOZflYj3i1M5nJs5gHhg/AXOXydIXAFaHTqYcg9DAOIwk5nJ06T1/SMfL8VT15rBaeNZo3H3wDwR8Gqcka5MAy3Y96Dn1ioYc6gRvX0pz7xnF8cvyIsHjk9PuEZ7C+Tfcfwa2bh9Qu1IqXCollqxO5jMnxwen3CPD42bp9wjfY+weTNdPckKAIg0piNqmMe3jKYdoofxROPJjngXlmcmauZljEGtB0NaQEmUoG1vNWtrC+20Zl85mNuYofFO25jP/NjbtoFys2+IxUgC5LD1p+EATvECpZFA9B+cZUsTuY81w5QjHpGcmOJ+cO53++Bnm8sYVTsaqAkkAdYzmZeKK1WVf+o7e3WCOVvRoM3zxJQqx9ANzGEzTM3xDVb5eFGw9epgWa+tizsSTyd4jpA22jrQfFoie8eAx5ENoKgeQVhZiq6sTYEG3Ubb8R0PJc5WmlmDKdm6evURzWQmplXYE39OYfqipZNuTz6+sKyJBwdm1zDMlShQV78H3O0V4HFzHoXTSCODfptSFeUT3sFQuh3qKAV3ubQ/efpFQLfeKdOsTONDuSqqK8ZodLi/exhPhMZOJZFAOnmlLcCHEnApiSH3p9oEAdiOYcy/D3R1A6BPv3jUvg5ySRzXJcmnT6LKls55IFh6tsvvGywH7MJjUM6aku11QF2+poPxjpWGwsuUgSWgRRsFAA+gi6Zt+kHLI/ApRRlMN+z3AoKsjzD/AFu1/wD8rQQ9wWSYeTeTIlIeoRa/XeDFBA3r3MQLEW/ysL5t9hUVrNa9TUiFfiHNBJlgC7vXSO1DUnttDTygMWGwJ9o5pm+YtOdnNgDQDovEDug4pXvoS50dbhgLsPP3IO570p9IWmv3U9ovx+Jq5FDbbbaKAepi7HaikyedOVo8CWNOn5wFMQdYMPNDFD3hiFsEdOhimhBqDeCpgEDsI6jA3C51MT5vMPoYbYfPEbc0PQ2jNFOYqcxjiEpM2gxQOxj5psYxJrL8pI94vTNJg3oYyjVI1JeIl4zwzv7QPtFy5sp5P0jKC5IdF4+1iFa41T/6iYxQjKOsZh4kGhYMTEjixHGNjhHET+IIzzZui8n6GKHz9RtU+0dQNmqE0R82JAjFzc+Y7D6wDOzKY+7n2tBcTLNxis2RBVmA97wixnikmyL7n9IzB7x4RG0jrDMRjnc1dq9uPpFYmAQPSPaRzSCU2gtJketMgQgwbluXTJ7aUAJ3qSAKbRnFdm85FYcc0ieHwzzW0opY9hYep2HvG0wHg2UFq7F2qLfKvpa59zDs5eEKrLAljooFIXLIl0Ek5dmRwHg1mI+K1K8KRUD1No08nw8krRpqaHd6MTTrUUHsIdy8MFA5PWB8bKdhRXIPUf3hEpyl2MUUuivF4lEFiDTft2hWuMV/ISQdXlP2TTg8gwZOleQavMxO5pCfFylDA8g1gewtG1y2SNPT+8O5K2hJk83UiMOaQ/SMpICTP//Z"}],                               
                        show:false
            },
            {
                title: "Florit's Amazing Shepered Pie >",
                slides: [{caption:"Usually prepared for", text:"Fancy dinners, where the friends haven't met in a while..", 
                        imageUri: "https://cdn.apartmenttherapy.info/image/fetch/f_auto,q_auto:eco,c_fill,g_auto,w_800,h_400/https%3A%2F%2Fstorage.googleapis.com%2Fgen-atmedia%2F3%2F2016%2F10%2Fa67abefd77353c737e3b9f76e8fec8c4925d198d.jpeg"},
                    {caption:"Ingredients", text:"6 medium red potatos | 1 kilo ground beef | 1 big white onion | 4 garlic cloves | 0.5 tp salt | 0.5 tp black ppeper | 0.25 tp cumin", 
                        imageUri:"https://cdn.apartmenttherapy.info/image/fetch/f_auto,q_auto:eco,c_fill,g_auto,w_800,h_400/https%3A%2F%2Fstorage.googleapis.com%2Fgen-atmedia%2F3%2F2012%2F11%2F7567828aa3c366ec6300999265923f4f9f3064b4.jpeg"},
                    {caption:"Instructions",text:`Chop the potato into medium size cubes and slice the onion into small pieces.
                                boil water in a pot, when they are boiling add the potato cubes, and let the cook for around 30 minutes (until the are very soft).
                                Start frying the meat , while frying it is recommended slice the meat with the spatula in order for not having lumps later, add the salt balck pepper and cumin while frying the meat.
                                fry the onion until it golden clear around 20 minutes on medium heat.
                                the potato should be ready by now, smash the good so there is no lumpers in the potato (almost like puree) and then add the onion.
                                when the meat is ready take a deep oven plate and put 1/3 of the puree and on top of that put 2/3 of the beef and than again 1/3 of puree and the rest of the meat and close it up with the rest amound of puree.
                                put it all in the oven at 180 degrees for around 10-15 minutes. Take it out when it looks good! and serve`, 
                        imageUri:"https://www.potatobusiness.com/wp-content/uploads/teaser-print.jpg"}],                
                        show:false
            }
        ]   
    };
  },
  created(){    
  }
};
</script>

<style lang="scss" scoped>
.title{
  font-family:Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}
.carousel{
    color: black !important; 
}
.openable :hover{
    cursor: pointer;
}
</style>
