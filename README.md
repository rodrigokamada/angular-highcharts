# Angular Highcharts


Application example built with [Angular](https://angular.io/) 14 and adding the charts using the [highcharts](https://www.npmjs.com/package/highcharts) library.

This tutorial was posted on my [blog](https://rodrigo.kamada.com.br/blog/adicionando-graficos-usando-a-biblioteca-highcharts-em-uma-aplicacao-angular) in portuguese and on the [DEV Community](https://dev.to/rodrigokamada/adding-charts-using-the-highcharts-library-to-an-angular-application-2bbl) in english.



[![Website](https://shields.braskam.com/v1/shields?name=website&format=rectangle&size=small&radius=5)](https://rodrigo.kamada.com.br)
[![LinkedIn](https://shields.braskam.com/v1/shields?name=linkedin&format=rectangle&size=small&radius=5)](https://www.linkedin.com/in/rodrigokamada)
[![Twitter](https://shields.braskam.com/v1/shields?name=twitter&format=rectangle&size=small&radius=5&socialAccount=rodrigokamada)](https://twitter.com/rodrigokamada)



## Prerequisites


Before you start, you need to install and configure the tools:

* [git](https://git-scm.com/)
* [Node.js and npm](https://nodejs.org/)
* [Angular CLI](https://angular.io/cli)
* IDE (e.g. [Visual Studio Code](https://code.visualstudio.com/))



## Getting started


### Create the Angular application


**1.** Let's create the application with the Angular base structure using the `@angular/cli` with the route file and the SCSS style format.

```shell
ng new angular-highcharts
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? SCSS   [ https://sass-lang.com/documentation/syntax#scss                ]
CREATE angular-highcharts/README.md (1063 bytes)
CREATE angular-highcharts/.editorconfig (274 bytes)
CREATE angular-highcharts/.gitignore (604 bytes)
CREATE angular-highcharts/angular.json (3279 bytes)
CREATE angular-highcharts/package.json (1080 bytes)
CREATE angular-highcharts/tsconfig.json (783 bytes)
CREATE angular-highcharts/.browserslistrc (703 bytes)
CREATE angular-highcharts/karma.conf.js (1435 bytes)
CREATE angular-highcharts/tsconfig.app.json (287 bytes)
CREATE angular-highcharts/tsconfig.spec.json (333 bytes)
CREATE angular-highcharts/src/favicon.ico (948 bytes)
CREATE angular-highcharts/src/index.html (303 bytes)
CREATE angular-highcharts/src/main.ts (372 bytes)
CREATE angular-highcharts/src/polyfills.ts (2820 bytes)
CREATE angular-highcharts/src/styles.scss (80 bytes)
CREATE angular-highcharts/src/test.ts (788 bytes)
CREATE angular-highcharts/src/assets/.gitkeep (0 bytes)
CREATE angular-highcharts/src/environments/environment.prod.ts (51 bytes)
CREATE angular-highcharts/src/environments/environment.ts (658 bytes)
CREATE angular-highcharts/src/app/app-routing.module.ts (245 bytes)
CREATE angular-highcharts/src/app/app.module.ts (393 bytes)
CREATE angular-highcharts/src/app/app.component.scss (0 bytes)
CREATE angular-highcharts/src/app/app.component.html (24617 bytes)
CREATE angular-highcharts/src/app/app.component.spec.ts (1109 bytes)
CREATE angular-highcharts/src/app/app.component.ts (223 bytes)
✔ Packages installed successfully.
```

**2.** Install and configure the Bootstrap CSS framework. Do steps 2 and 3 of the post *[Adding the Bootstrap CSS framework to an Angular application](https://github.com/rodrigokamada/angular-bootstrap)*.

**3.** Install the `highcharts` library.

```shell
npm install highcharts
```

**4.** Remove the contents of the `AppComponent` class from the `src/app/app.component.ts` file. Import the `Highcharts`, `HighchartsMore` and `HighchartsSolidGauge` services and create the `createChartGauge`, `createChartPie`, `createChartColumn` and `createChartLine` methods as below.

```typescript
import { AfterViewInit, Component } from '@angular/core';
import * as Highcharts from 'highcharts';
import HighchartsMore from 'highcharts/highcharts-more';
import HighchartsSolidGauge from 'highcharts/modules/solid-gauge';

HighchartsMore(Highcharts);
HighchartsSolidGauge(Highcharts);

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
})
export class AppComponent implements AfterViewInit {

  public ngAfterViewInit(): void {
    this.createChartGauge();
    this.createChartPie();
    this.createChartColumn();
    this.createChartLine();
  }

  private getRandomNumber(min: number, max: number): number {
    return Math.floor(Math.random() * (max - min + 1) + min)
  }

  private createChartGauge(): void {
    const chart = Highcharts.chart('chart-gauge', {
      chart: {
        type: 'solidgauge',
      },
      title: {
        text: 'Gauge Chart',
      },
      credits: {
        enabled: false,
      },
      pane: {
        startAngle: -90,
        endAngle: 90,
        center: ['50%', '85%'],
        size: '160%',
        background: {
            innerRadius: '60%',
            outerRadius: '100%',
            shape: 'arc',
        },
      },
      yAxis: {
        min: 0,
        max: 100,
        stops: [
          [0.1, '#55BF3B'], // green
          [0.5, '#DDDF0D'], // yellow
          [0.9, '#DF5353'], // red
        ],
        minorTickInterval: null,
        tickAmount: 2,
        labels: {
          y: 16,
        },
      },
      plotOptions: {
        solidgauge: {
          dataLabels: {
            y: -25,
            borderWidth: 0,
            useHTML: true,
          },
        },
      },
      tooltip: {
        enabled: false,
      },
      series: [{
        name: null,
        data: [this.getRandomNumber(0, 100)],
        dataLabels: {
          format: '<div style="text-align: center"><span style="font-size: 1.25rem">{y}</span></div>',
        },
      }],
    } as any);

    setInterval(() => {
      chart.series[0].points[0].update(this.getRandomNumber(0, 100));
    }, 1000);
  }

  private createChartPie(): void {
    let date = new Date();
    const data: any[] = [];

    for (let i = 0; i < 5; i++) {
      date.setDate(new Date().getDate() + i);
      data.push({
        name: `${date.getDate()}/${date.getMonth() + 1}`,
        y: this.getRandomNumber(0, 1000),
      });
    }

    const chart = Highcharts.chart('chart-pie', {
      chart: {
        type: 'pie',
      },
      title: {
        text: 'Pie Chart',
      },
      credits: {
        enabled: false,
      },
      tooltip: {
        headerFormat: `<span class="mb-2">Date: {point.key}</span><br>`,
        pointFormat: '<span>Amount: {point.y}</span>',
        useHTML: true,
      },
      series: [{
        name: null,
        innerSize: '50%',
        data,
      }],
    } as any);

    setInterval(() => {
      date.setDate(date.getDate() + 1);
      chart.series[0].addPoint({
        name: `${date.getDate()}/${date.getMonth() + 1}`,
        y: this.getRandomNumber(0, 1000),
      }, true, true);
    }, 1500);
  }

  private createChartColumn(): void {
    let date = new Date();
    const data: any[] = [];

    for (let i = 0; i < 10; i++) {
      date.setDate(new Date().getDate() + i);
      data.push({
        name: `${date.getDate()}/${date.getMonth() + 1}`,
        y: this.getRandomNumber(0, 1000),
      });
    }

    const chart = Highcharts.chart('chart-column' as any, {
      chart: {
        type: 'column',
      },
      title: {
        text: 'Column Chart',
      },
      credits: {
        enabled: false,
      },
      legend: {
        enabled: false,
      },
      yAxis: {
        min: 0,
        title: undefined,
      },
      xAxis: {
        type: 'category',
      },
      tooltip: {
        headerFormat: `<div>Date: {point.key}</div>`,
        pointFormat: `<div>{series.name}: {point.y}</div>`,
        shared: true,
        useHTML: true,
      },
      plotOptions: {
        bar: {
          dataLabels: {
            enabled: true,
          },
        },
      },
      series: [{
        name: 'Amount',
        data,
      }],
    } as any);

    setInterval(() => {
      date.setDate(date.getDate() + 1);
      chart.series[0].addPoint({
        name: `${date.getDate()}/${date.getMonth() + 1}`,
        y: this.getRandomNumber(0, 1000),
      }, true, true);
    }, 1500);
  }

  private createChartLine(): void {
    let date = new Date();
    const data: any[] = [];

    for (let i = 0; i < 10; i++) {
      date.setDate(new Date().getDate() + i);
      data.push([`${date.getDate()}/${date.getMonth() + 1}`, this.getRandomNumber(0, 1000)]);
    }

    const chart = Highcharts.chart('chart-line', {
      chart: {
        type: 'line',
      },
      title: {
        text: 'Line Chart',
      },
      credits: {
        enabled: false,
      },
      legend: {
        enabled: false,
      },
      yAxis: {
        title: {
          text: null,
        }
      },
      xAxis: {
        type: 'category',
      },
      tooltip: {
        headerFormat: `<div>Date: {point.key}</div>`,
        pointFormat: `<div>{series.name}: {point.y}</div>`,
        shared: true,
        useHTML: true,
      },
      series: [{
        name: 'Amount',
        data,
      }],
    } as any);

    setInterval(() => {
      date.setDate(date.getDate() + 1);
      chart.series[0].addPoint([`${date.getDate()}/${date.getMonth() + 1}`, this.getRandomNumber(0, 1000)], true, true);
    }, 1500);
  }

}
```

**5.** Remove the contents of the `src/app/app.component.html` file. Add the `div` tags for each chart as below.

```html
<div class="container-fluid py-3">
  <h1>Angular Highcharts</h1>

  <div class="row mt-5">
    <div class="col-6">
      <div id="chart-gauge"></div>
    </div>
    <div class="col-6">
      <div id="chart-pie"></div>
    </div>
  </div>
  <div class="row mt-5">
    <div class="col-6">
      <div id="chart-column"></div>
    </div>
    <div class="col-6">
      <div id="chart-line"></div>
    </div>
  </div>
</div>
```

**6.** Add the style in the `src/app/app.component.scss` file as below.

```css
#chart-gauge, #chart-pie, #chart-column, #chart-line {
  display: block;
  min-width: 150px;
  max-width: 400px;
  height: 200px;
  margin: 0 auto;
}
```

**7.** Run the application with the command below.

```shell
npm start

> angular-highcharts@1.0.0 start
> ng serve

✔ Browser application bundle generation complete.

Initial Chunk Files | Names         |      Size
vendor.js           | vendor        |   2.79 MB
styles.css          | styles        | 266.63 kB
polyfills.js        | polyfills     | 128.52 kB
scripts.js          | scripts       |  77.06 kB
main.js             | main          |  18.40 kB
runtime.js          | runtime       |   6.64 kB

                    | Initial Total |   3.28 MB

Build at: 2021-09-09T01:25:33.291Z - Hash: 2495029c373d88bd55c1 - Time: 27953ms

** Angular Live Development Server is listening on localhost:4200, open your browser on http://localhost:4200/ **


✔ Compiled successfully.
```

**8.** Ready! Access the URL `http://localhost:4200/` and check if the application is working. See the application working on [GitHub Pages](https://rodrigokamada.github.io/angular-highcharts/) and [Stackblitz](https://stackblitz.com/edit/angular14-highcharts).

![Angular Highcharts](https://res.cloudinary.com/rodrigokamada/image/upload/v1638101045/Blog/angular-highcharts/angular-highcharts.gif)



## Cloning the application

**1.** Clone the repository.

```shell
git clone git@github.com:rodrigokamada/angular-highcharts.git
```

**2.** Install the dependencies.

```shell
npm ci
```

**3.** Run the application.

```shell
npm start
```