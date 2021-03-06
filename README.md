# PolidogControllerFilterBundle

This bundle to allow you to describe filtering with annotations in the Controller class.

## Install
   
install polidog/controller-filter-bundle with composer.

```
$ composer require polidog/controller-filter-bundle "@dev"
```

## Configuration

```
// AppKernel.php
public function registerBundles()
{
    $bundles = array(
        // ...
        new Polidog\ControllerFilterBundle\PolidogControllerFilterBundle(),
        // ...
    );
}
```

## Using

```
// controller class

/**
 * @Route("/card")
 * @Filter(Filter::TYPE_BEFORE, method="checkSession", service="app.service.check_service")
 */
class CardController extends Controller
{
    use DonationSessionTrait;

    /**
     * @Route("/")
     * @Method("GET")
     * @Template()
     * @Filter(Filter::TYPE_AFTER, method="changeResult", service="app.service.check_service")
     */
    public function indexAction(Request $request)
    {
        return ['hoge' =>'fuga'];
    }



}

```

```
// service class
class CheckService {
    /** 
     * @Session
     * /
    private $session;
    
    
    public function changeResult()
    {
        if ($this->session->has('hoge') {
            throw new \Exception();
        }
    }
    
    public function changeResult(GetResponseForControllerResultEvent $event)
    {
        $event->setControllerResult(['hoge' => 'hogehoge']);
    }    
}

```

```
// services.yml

services:
    app.service.check_service:
        class: AppBundle\Service\CheckService
        arguments: ["@session"]

```

