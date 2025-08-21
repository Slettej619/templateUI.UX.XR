You are given a task to integrate an existing React component in the codebase

The codebase should support:
- shadcn project structure  
- Tailwind CSS
- Typescript

If it doesn't, provide instructions on how to setup project via shadcn CLI, install Tailwind or Typescript.

Determine the default path for components and styles. 
If default path for components is not /components/ui, provide instructions on why it's important to create this folder
Copy-paste this component to /components/ui folder:
```tsx
hero-video.tsx
"use client"

import * as React from "react"
import {
  HTMLMotionProps,
  MotionValue,
  motion,
  useMotionTemplate,
  useScroll,
  useTransform,
} from "motion/react"

import { cn } from "@/lib/utils"

type AnimateT = "left" | "right" | "top" | "bottom" | "z" | "blur" | undefined
const SPRING_CONFIG = {
  type: "spring",
  stiffness: 100,
  damping: 16,
  mass: 0.75,
  restDelta: 0.005,
  duration: 0.3,
}
const useAnimationVariants = (animate: AnimateT) =>
  React.useMemo(
    () => ({
      hidden: {
        x: animate === "left" ? "-100%" : animate === "right" ? "100%" : 0,
        y: animate === "top" ? "-100%" : animate === "bottom" ? "100%" : 0,
        scale: animate === "z" ? 0 : 1,
        filter: animate === "blur" ? "blur(10px)" : "blur(0px)",
        opacity: 0,
      },
      visible: {
        x: 0,
        y: 0,
        scale: 1,
        filter: "blur(0px)",
        opacity: 1,
      },
    }),
    [animate]
  )

const ContainerStagger = React.forwardRef<
  HTMLDivElement,
  HTMLMotionProps<"div">
>(({ children, className, ...props }, ref) => {
  return (
    <motion.div
      className={cn("relative", className)}
      ref={ref}
      initial="hidden"
      whileInView={"visible"}
      viewport={{ once: true || props.viewport?.once, ...props.viewport }}
      transition={{
        staggerChildren: props.transition?.staggerChildren || 0.2,
        ...props.transition,
      }}
      {...props}
    >
      {children}
    </motion.div>
  )
})
ContainerStagger.displayName = "ContainerStagger"

interface ContainerAnimatedProps extends HTMLMotionProps<"div"> {
  animation?: AnimateT
}
interface ContainerScrollValue {
  scrollYProgress: MotionValue<number>
}
const ContainerScrollContext = React.createContext<
  ContainerScrollValue | undefined
>(undefined)
function useContainerScrollContext() {
  const context = React.useContext(ContainerScrollContext)
  if (!context) {
    throw new Error(
      "useContainerScrollContext must be used within <ContainerScroll> component"
    )
  }
  return context
}
interface ContainerScrollProps extends React.HTMLAttributes<HTMLDivElement> {}

const ContainerScroll = ({
  children,
  className,
  ...props
}: ContainerScrollProps) => {
  const scrollRef = React.useRef<HTMLDivElement>(null)
  const { scrollYProgress } = useScroll({
    target: scrollRef,
  })

  return (
    <ContainerScrollContext.Provider value={{ scrollYProgress }}>
      <section
        className={cn(
          "relative min-h-[120vh] w-full pb-[30%] pt-8 ",
          className
        )}
        {...props}
        ref={scrollRef}
      >
        {children}
      </section>
    </ContainerScrollContext.Provider>
  )
}
ContainerScroll.displayName = "ContainerScroll"
const ContainerAnimated = React.forwardRef<
  HTMLDivElement,
  ContainerAnimatedProps
>(({ animation, children, className, ...props }, ref) => {
  const variants = useAnimationVariants(animation)

  return (
    <motion.div
      transition={SPRING_CONFIG || props.transition}
      ref={ref}
      variants={variants}
      className={className}
      {...props}
    >
      {children}
    </motion.div>
  )
})
ContainerAnimated.displayName = "ContainerAnimated"

interface ContainerInsetProps extends HTMLMotionProps<"div"> {
  translateYRange?: [string, string]
  insetYRange?: [number, number]
  insetXRange?: [number, number]
  roundednessRange?: [number, number]
}
const ContainerInset = React.forwardRef<HTMLDivElement, ContainerInsetProps>(
  (
    {
      translateYRange = ["-25%", "50%"],
      insetYRange = [35, 0],
      insetXRange = [42, 0],
      roundednessRange = [1000, 16],
      children,
      className,
      ...props
    },
    ref
  ) => {
    const { scrollYProgress } = useContainerScrollContext()
    const y = useTransform(scrollYProgress, [0, 1], translateYRange)

    const insetY = useTransform(scrollYProgress, [0, 1], insetYRange)
    const insetX = useTransform(scrollYProgress, [0, 1], insetXRange)
    const roundedness = useTransform(scrollYProgress, [0, 1], roundednessRange)

    const clipPath = useMotionTemplate`inset(${insetY}% ${insetX}% ${insetY}% ${insetX}% round ${roundedness}px)`

    const style = React.useMemo(
      () => ({ y, clipPath, ...props.style }),
      [y, clipPath, props.style]
    )
    return (
      <motion.div
        transition={SPRING_CONFIG || props.transition}
        ref={ref}
        className={cn("origin-top overflow-hidden", className)}
        style={style}
        {...props}
      >
        {children}
      </motion.div>
    )
  }
)
ContainerInset.displayName = "ContainerInset"

export { ContainerAnimated, ContainerStagger, ContainerScroll, ContainerInset }

demo.tsx
import { ContainerAnimated,
  ContainerInset,
  ContainerScroll,
  ContainerStagger, } from "@/components/blocks/hero-video"
import { Button } from "@/components/ui/button"

const DemoVariant1 = () => {
  return (
    <ContainerScroll className="bg-stone-100 text-center text-stone-800">
      <ContainerStagger viewport={{ once: false }}>
        <ContainerAnimated animation="top">
          <h1 className=" text-4xl font-black leading-none tracking-tighter">
            Wait! what is this?,
          </h1>
        </ContainerAnimated>
        <ContainerAnimated animation="bottom">
          <h1 className=" text-4xl font-black leading-none tracking-tighter">
            Scroll and see.
          </h1>
        </ContainerAnimated>

        <ContainerAnimated animation="blur" className="my-4">
          <p className=" text-xl leading-normal tracking-tight">
            🎯 One component with diffrent animations,
            <br />
            ✨ using &quot;animation&quot; prop to animate the component.,
            <br />
            🪄 opacity(default) top bottom left right blur z.
          </p>
        </ContainerAnimated>

        <ContainerAnimated
          animation="blur"
          className="flex justify-center gap-2"
        >
          <Button
            variant={"secondary"}
            className="rounded-full bg-stone-500 text-white hover:bg-stone-400"
            size="lg"
          >
            Get Started
          </Button>
          <Button
            variant={"outline"}
            className="rounded-full border-stone-500 text-stone-600 hover:bg-stone-300"
            size="lg"
          >
            Learn more
          </Button>
        </ContainerAnimated>
      </ContainerStagger>

      <ContainerInset className=" mx-8">
        <video
          width="100%"
          height="100%"
          loop
          playsInline
          autoPlay
          muted
          className="relative z-10 block h-auto max-h-full max-w-full object-contain align-middle"
        >
          <source
            src="https://videos.pexels.com/video-files/8084758/8084758-uhd_2560_1440_25fps.mp4"
            type="video/mp4"
          />
        </video>
      </ContainerInset>
    </ContainerScroll>
  )
}

const DemoVariant2 = () => {
  return (
    <ContainerScroll
      style={{
        background: "#434343",
        backgroundImage: "linear-gradient(to right, #434343 0%, #000 100%)",
      }}
      className="text-center text-stone-100"
    >
      <ContainerStagger>
        <ContainerAnimated animation="top">
          <h1 className=" text-4xl font-black leading-none tracking-tighter">
            Wait! what is this?,
          </h1>
        </ContainerAnimated>
        <ContainerAnimated animation="top">
          <h1 className=" text-4xl font-black leading-none tracking-tighter">
            Scroll and see.
          </h1>
        </ContainerAnimated>

        <ContainerAnimated animation="z" className="my-4">
          <p className=" text-xl leading-normal tracking-tight">
            🎯 One component with diffrent animations,
            <br />
            ✨ using &quot;animation&quot; prop to animate the component.,
            <br />
            🪄 opacity(default) top bottom left right blur z.
          </p>
        </ContainerAnimated>

        <ContainerAnimated
          animation="bottom"
          className="flex justify-center gap-2"
        >
          <Button
            variant={"secondary"}
            className="rounded-full bg-indigo-400 text-white hover:bg-indigo-300"
            size="lg"
          >
            Get Started
          </Button>
          <Button
            variant={"outline"}
            className="rounded-full border-indigo-300 text-indigo-300 bg-transparent hover:bg-indigo-200"
            size="lg"
          >
            Learn more
          </Button>
        </ContainerAnimated>
      </ContainerStagger>

      <ContainerInset insetXRange={[30, 0]} className=" mx-8">
        <video
          width="100%"
          height="100%"
          loop
          playsInline
          autoPlay
          muted
          className="relative z-10 block h-auto max-h-full max-w-full object-contain align-middle"
        >
          <source
            src="https://videos.pexels.com/video-files/8086707/8086707-uhd_2560_1440_25fps.mp4"
            type="video/mp4"
          />
        </video>
      </ContainerInset>
    </ContainerScroll>
  )
}
 
export { DemoVariant1, DemoVariant2 }
```

Copy-paste these files for dependencies:
```tsx
shadcn/button
import * as React from "react"
import { Slot } from "@radix-ui/react-slot"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"

const buttonVariants = cva(
  "inline-flex items-center justify-center whitespace-nowrap rounded-md text-sm font-medium ring-offset-background transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:bg-primary/90",
        destructive:
          "bg-destructive text-destructive-foreground hover:bg-destructive/90",
        outline:
          "border border-input bg-background hover:bg-accent hover:text-accent-foreground",
        secondary:
          "bg-secondary text-secondary-foreground hover:bg-secondary/80",
        ghost: "hover:bg-accent hover:text-accent-foreground",
        link: "text-primary underline-offset-4 hover:underline",
      },
      size: {
        default: "h-10 px-4 py-2",
        sm: "h-9 rounded-md px-3",
        lg: "h-11 rounded-md px-8",
        icon: "h-10 w-10",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  },
)

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, asChild = false, ...props }, ref) => {
    const Comp = asChild ? Slot : "button"
    return (
      <Comp
        className={cn(buttonVariants({ variant, size, className }))}
        ref={ref}
        {...props}
      />
    )
  },
)
Button.displayName = "Button"

export { Button, buttonVariants }

```

Install NPM dependencies:
```bash
motion, @radix-ui/react-slot, class-variance-authority
```

Implementation Guidelines
 1. Analyze the component structure and identify all required dependencies
 2. Review the component's argumens and state
 3. Identify any required context providers or hooks and install them
 4. Questions to Ask
 - What data/props will be passed to this component?
 - Are there any specific state management requirements?
 - Are there any required assets (images, icons, etc.)?
 - What is the expected responsive behavior?
 - What is the best place to use this component in the app?

Steps to integrate
 0. Copy paste all the code above in the correct directories
 1. Install external dependencies
 2. Fill image assets with Unsplash stock images you know exist
 3. Use lucide-react icons for svgs or logos if component requires them
