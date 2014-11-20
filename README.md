Phan-Proxy
==========




import logging
from selenium import webdriver
import logging
from  random  import choice
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities
from selenium.webdriver.support.ui import Select
from selenium.webdriver.support.ui import WebDriverWait
from selenium.common.exceptions import WebDriverException

logging.basicConfig(level=logging.DEBUG, format='[%(levelname)s] (%(threadName)-10s) %(message)s', )



def main(link):
    f2 = open("/home/desktop/proxy_http_auth.txt")
    #f2 = open("/home/user/Desktop/anit/naptol/new_proxy.txt")
    proxy_list = f2.read().strip().split("\n")
    f2.close()

    for l in xrange(6):
        ip_port = choice(proxy_list).strip()
        logging.debug(ip_port)

        user_pass = ip_port.split("@")[0].strip()
        prox = "--proxy=%s" % (ip_port.split("@")[1].strip())
       
        #prox = "--proxy=%s8080" % (ip_port.strip())
        #prox = "--proxy=%s" % (ip_port.strip())
        service_args = [prox, '--proxy-auth='+user_pass, '--proxy-type=http', '--load-images=no']
        
        #service_args = [prox,  '--proxy-type=http', '--load-images=no']
        dcap = dict(DesiredCapabilities.PHANTOMJS)

        dcap["phantomjs.page.settings.userAgent"] = ("Mozilla/5.0 (X11; Ubuntu; Linux i686; rv:23.0) Gecko/20100101 Firefox/23.0")
        dcap["--disable-popup-blocking"] = False

        driver = webdriver.PhantomJS(service_args = service_args, desired_capabilities=dcap)
        driver.maximize_window()

        driver.implicitly_wait(5)
        driver.set_page_load_timeout(25)
       
 
        #driver = driver.find_element_by_xpath("/html/body/div[3]/div/div/a").click()    
        try:
            driver.get(link)

        except:
            pass            


        if str(driver.current_url).strip() != "about:blank":
            return  driver

        else:
            driver.delete_all_cookies()
            driver.quit()


    return None




if __name__=="__main__":
    link = "http://www.naaptol.com/mobile-handsets/sony-xperia-c-c2305/p/12292722.html"
    driver = main(link)
    page = driver.page_source
    print page
                 
